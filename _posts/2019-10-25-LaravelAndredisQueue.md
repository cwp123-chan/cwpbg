---
title: laravel使用redis配置队列
date: 2019-10-25 23:40:49
---
# laravel使用redis配置队列

有些任务并不需要及时运行，就可以将其写入队列，从而不影响主业务逻辑的进程。如：用户发帖成功后推送消息给其关注的用户。如果一个用户是大v，有几百万的粉丝，肯定不能将发贴与推送通知的逻辑捆绑在一起，不然分分钟卡死。

![img](https:////upload-images.jianshu.io/upload_images/1864602-43d1aa1f7a01d256.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/704/format/webp)

image

以下用一个场景来模拟队列：

## 模拟用户发贴

新建路由：`routes/web.php`

```php
Route::get('/publish-article', 'HomeController@publish')->name('home.publish-article');
```

浏览器访问 `http://image_lib.airmb.test/publish-article`

![img](https:////upload-images.jianshu.io/upload_images/1864602-b0b2d1a40d7991dd.png?imageMogr2/auto-orient/strip|imageView2/2/w/349/format/webp)

image

## 配置redis队列

每当用户成功发贴，就将这一事件写入队列，我们使用Redis作为队列驱动器

首先安装相应扩展 [nrk/predis](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fnrk%2Fpredis)

```bash
$ composer require predis/predis
```

修改 `.env` 的 `QUEUE_CONNECTION` 值

```php
QUEUE_CONNECTION=redis
```

## 任务失败重试表

有时候队列中的任务会失败。Laravel 内置了一个方便的方式来指定任务重试的最大次数。当任务超出这个重试次数后，它就会被插入到 failed_jobs 数据表里面。我们可以使用 queue:failed-table 命令来创建 failed_jobs 表的迁移文件：

```bash
$ php artisan queue:failed-table
```

生成 `failed_jobs` 表：

```bash
$ php artisan migrate
```

## 生成任务类

```bash
$ php artisan make:job Notice
Job created successfully.
```

自动生成 `app/Jobs/Notice.php`，将该文件改为：

```php
namespace App\Jobs;
use App\User;
use Illuminate\Bus\Queueable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
class Notice implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;
    protected $user;
    public function __construct(User $user)
    {
        $this->user = $user;
    }
    public function handle()
    {
        $content = $this->user->name . "发表了新文章" . time() . "\n";
        file_put_contents('./notice.txt', $content, FILE_APPEND);
    }
}
```

## 任务分发

在 `app/Http/Controllers/HomeController.php` 进行任务分发：

```php
# ...
use App\User;
# ...
public function publish(User $user)
{
    $user = User::query()->first();
    $this->dispatch(new Notice($user));
    dd('文章发布成功');
}

```

```
   use App\User;

   public function publish(User $user)
    {
        $k = 0;
            do{
                $k++;
                $user = User::find($k);
                $this->dispatch(new Notice($user));
            }while($k);


        dd('文章发布成功');
        Redis::pipeline(function($pipe){
            for ($i = 0 ; $i < 1000;$i++) {
                $pipe->set("key:$i", $i);
            }
        });

    }
```



## 测试

命令行开启队列监听：

```bash
$ php artisan queue:listen
```

浏览器访问：`http://image_lib.airmb.test/publish-article`

```bash
$ php artisan queue:listen
[2019-03-27 16:23:02][2184] Processing: App\Jobs\Notice
[2019-03-27 16:23:02][2184] Processed:  App\Jobs\Notice
```

结果：

```bash
$ cat ./notice.txt
test发表了新文章1553674982
```

任务运行失败时，会显示 `failed`

```bash
$ php artisan queue:listen
[2019-03-27 16:23:02][2184] Processing: App\Jobs\Notice
[2019-03-27 16:23:02][2184] Processed:  App\Jobs\Notice
```

到数据表`failed_job`查看程序的详细报错