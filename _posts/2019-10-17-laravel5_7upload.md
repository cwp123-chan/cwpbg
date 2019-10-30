---
title: laravel 5.7 上传文件
date: 2019-10-20 23:40:49
---
## laravel 5.7 上传文件

### 1. 前台

```html
// 利用ant-desigin 
 <template>
            <div>
              <a-upload
                listType="picture"
                :defaultFileList="fileList"
                :customRequest = "uploadPic"
              >
                <a-button> <a-icon type="upload" /> upload </a-button>
              </a-upload>
              <br />
            </div>
          </template>
```

vue

```js
import qs from "qs";
    import axios from "axios";
    import apiConfig from '../../router/httpConfig.js'
    export default {
        name: "editProductClass",
        data() {
            return {
                fileList: [
                ],
            };
        },
        methods:{

            uploadPic(data){
                let config = {
                    headers: {
                        "Content-Type": "multipart/form-data"
                    }
                };
                let form = new FormData();
                form.append("name",data.file);
                console.log(form.get("name"))
                axios
                    .post(apiConfig.uploadPicData,form,config)
                    .then(res=>{
                        console.log(res)
                    })
            }
        }

    }
```

css

```css
 /* tile uploaded pictures */
   .upload-list-inline >>> .ant-upload-list-item {
     float: left;
     width: 200px;
     margin-right: 8px;
   }
  .upload-list-inline >>> .ant-upload-animate-enter {
    animation-name: uploadAnimateInlineIn;
  }
  .upload-list-inline >>> .ant-upload-animate-leave {
    animation-name: uploadAnimateInlineOut;
  }
```

laravel	第一种

```php
 public function picUpload(Request $request){
        if ($request->isMethod('POST')) {
            $fileChar = $request->file("name");
            if($fileChar->isValid()){
                //获取文件的扩展名
                $ext = $fileChar->getClientOriginalExtension();
                //获取文件的绝对路径
                $path = $fileChar->getRealPath();
                //定义文件名
                $filename = date('Y-m-d-h-i-s').'.'.$ext;
                //存储文件。disk里面的public。总的来说，就是调用disk模块里的public配置
                // 获取 $path 临时文件夹下的图片 上传到 laravel 中 storage/app/public/目录下
                Storage::disk('public')->put($filename, file_get_contents($path));
                //storage_path函数返回storage目录的绝对路径：
//                $path = storage_path();
                //还可以使用storage_path函数生成相对于storage目录的给定文件的绝对路径：
                $path = storage_path('app/public'.$filename);
                return $path;
            }
        }

    }
```

后台 第二种方法 返回key 的操作 (模拟token为 md5(日期)) 

在 laravel 5.7 中 127.0.0.1 所 对应的文件 地址 是 在 项目根目录下的public文件中

正式中 使用 的key与token 以实际为出发点

```php
// 传入临时文件夹 返回key
public $url = "127.0.0.1"  //设置laravel的域名访问路径
public function picUpload(Request $request){
        // 判断前台数据是否为post
        if ($request->isMethod('POST')) {
            $fileChar = $request->file("name");
            if($fileChar->isValid()){
                //获取文件的扩展名
                $ext = $fileChar->getClientOriginalExtension();
                //获取文件的绝对路径
                $path = $fileChar->getRealPath();
                // 假设一个token值
                $token = md5(date('Y-m-d'));
                //定义文件名
                $filename = date('Y-m-d-h-i-s').'.'.$ext;
                // 定义 新地址的目录路径
                $dirname = date('Y/m/d/h/');
                if(is_dir('./tagControllerImgTmp/'.$token.'/'.$dirname)){
                    $newDir = './tagControllerImgTmp/'.$token.'/'.$dirname;
                    $returnDir = '/tagControllerImgTmp/'.$token.'/'.$dirname.$filename;
                    // 单个图片的key值
                    $key = md5($newDir.$filename);
                    // 启用 redis;
                    Redis::set($key,$newDir.$filename);
                    $values = Redis::get($key);
                    // 将 临时文件移到 自己定义的文件夹中 并返回给前端 key
                    copy($path,$newDir.$filename);
                    return [
                        "status"=>"true",
                        "key"=>$key,
                        "value"=>$values,
                        "tmp"=>(new tagController)->url.$returnDir
                    ];
                }else{
                    mkdir('./tagControllerImgTmp/'.$token.'/'.$dirname,0700,true);
                    $newDir = './tagControllerImgTmp/'.$token.'/'.$dirname;
                    $returnDir = '/tagControllerImgTmp/'.$token.'/'.$dirname.$filename;
                    // 单个图片的key值
                    $key = md5($newDir.$filename);
                    Redis::set($key,$newDir.$filename);
                    $values = Redis::get($key);
                    copy($path,$newDir.$filename);
                    return [
                        "status"=>"true",
                        "key"=>$key,
                        "tmp"=>(new tagController)->url.$returnDir
                    ];
                }
            }else{
                return [
                    "status"=>"false",
                    "msg" =>"文件不存在"
                ];
            }
        }else{
            return [
              "status"=>"false",
                "msg" =>"不支持的传输类型"
            ];
        }
    }

 // 加入正式文件夹 存入路径给数据库
    public function pushPicUpload(Request $request){
        if(empty($request->all())){
            return [
                "status"=>"false",
                "msg" => "请输入值"
            ];
        }else{
            $theNewPathAll = [];
            //循环查询前端的key是否存在redis中 并创建新的文件目录启用 文件目录后 传到数据库中
            for ($i = 0; $i < count($request->all()["key"]); $i++){
                if(($request->all()["key"])[$i]){
                    $oldPath = Redis::get(($request->all()["key"])[$i]);
                    $tokenPath = explode("/",explode("./tagControllerImgTmp/",$oldPath)[1]);
                    $picSuffix = $tokenPath[5];
                    $tokenPath[5] = "";
                    $tokenPath = implode("/",$tokenPath);
                    if(!empty($oldPath)){
                        if(is_dir('./tagControllerImgReal/'.$tokenPath)){
                            $newPicPath = './tagControllerImgReal/'.$tokenPath.$picSuffix;
                            copy($oldPath,$newPicPath);
                            $theNewPathAll[$i] = $newPicPath;
                        }else{
                            mkdir('./tagControllerImgReal/'.$tokenPath,0700,true);
                            $newPicPath = './tagControllerImgReal/'.$tokenPath.$picSuffix;
                            copy($oldPath,$newPicPath);
                            $theNewPathAll[$i] = $newPicPath;
                        }

                    }else{
                        return [
                          "status"=>"false",
                          "msg" => "key值不正确"
                        ];
                    }
                }else{
                    return [
                        "status"=>"false",
                        "msg"=>"请传入正确的key值"
                    ];
                }
            }
            if($request->all()["func"] == "create"){
                // 判断前台的操作 如果为 床架 就进入创建表
                return '我是创建';
            }else if($request->all()["func"] == "upload"){
                // 如果是 更新 就进入更新表
                return $theNewPathAll;
            }else{
                return [
                    "status"=>"false",
                    "msg" => "创建失败"
                ];
            }
        }

    }
```







### laravel 常用 获取地址的方法

```
1.app_path()

app_path函数返回app目录的绝对路径：
$path = app_path();

你还可以使用app_path函数为相对于app目录的给定文件生成绝对路径：
$path = app_path('Http/Controllers/Controller.php');

2.base_path()

base_path函数返回项目根目录的绝对路径：
$path = base_path();

你还可以使用base_path函数为相对于应用目录的给定文件生成绝对路径：
$path = base_path('vendor/bin');

3.config_path()

config_path函数返回应用配置目录的绝对路径：
$path = config_path();

4.database_path()

database_path函数返回应用数据库目录的绝对路径：
$path = database_path();

5.public_path()

public_path函数返回public目录的绝对路径：
$path = public_path();

6.storage_path()

storage_path函数返回storage目录的绝对路径：
$path = storage_path();

还可以使用storage_path函数生成相对于storage目录的给定文件的绝对路径：
$path = storage_path('app/file.txt');
```

