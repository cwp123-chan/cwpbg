---
title: php操作mongodb
date: 2019-10-25 23:50:49
---
# php操作mongodb

连接

```
$manager = new MongoDB\Driver\Manager('mongodb://tanchun:123456@localhost:27017');
$bulk = new MongoDB\Driver\BulkWrite; //默认是有序的，串行执行
//$bulk = new MongoDB\Driver\BulkWrite(['ordered' => flase]);//如果要改成无序操作则加flase，并行执行
```

增

```
$bulk->insert(['user_id'=>1,'name'=>"李刚","position"=>"test engineer","age"=>20]);
$bulk->insert(['user_id'=>2,'name'=>"侯伟","position"=>"web engineer","age"=>20]);
$bulk->insert(['user_id'=>3,'name'=>"陈发","position"=>"php engineer","age"=>20]);
$status = $manager->executeBulkWrite('admin.colleagues', $bulk); //执行写入 location数据库下的colleagues集合
 
//插入后的结果：
> db.colleagues.find()
{ "_id":ObjectId("5bc416339dc6d604d715a0d2"),"user_id":1,"name":"李刚","position":"test engineer","age":20 }
{ "_id":ObjectId("5bc416339dc6d604d715a0d3"),"user_id":2,"name":"侯伟","position":"web engineer","age":20 }
{ "_id":ObjectId("5bc416339dc6d604d715a0d4"),"user_id":3,"name":"陈发","position":"php engineer","age":20 }  
```

删

```
$bulk->delete(['user_id'=>3]);
$status = $manager->executeBulkWrite('admin.colleagues',$bulk);
 
$bulk->delete(['user_id' => 1], ['limit' => 1]);   // limit 为 1 时，删除第一条匹配数据
$bulk->delete(['user_id' => 2], ['limit' => 0]);   // limit 为 0 时，删除所有匹配数据，默认删除所有
$status = $manager->executeBulkWrite('admin.colleagues',$bulk);
```

改

```
/*
 * 如果字段不存在会添加一个字段
 * 如果条件不成立，则新增数据，如果要设置条件不成立不增加可以设置'upsert' => false
 * multi默认为true,表示满足条件全部修改，false表示只修改满足条件的第一条
 */
$bulk->update(
    ['user_id'=>2],
    ['$set'=>
        [
        'name'=>'侯4',
        'age'=>15,
        'real_name'=>'侯4'
        ]
    ],
    ['multi'=>true,'upsert'=>false]
);
$status = $manager->executeBulkWrite('admin.colleagues',$bulk);
```

查

```
$filter =  [
   'user_id'=>['$gt'=>0],
   '$or' => [
        ['sys'=>'ios'],
        ['sys'=>'other']
    ],
    'source' => [
        '$in' => ['bdsem1', 'bdsem2', 'bdsem3']
    ],
    //-------------------
    /*'$group' => [
        '_id' => '$sys',
        'total' => ['$sum'=>1]
    ]*/
    // mysql: SELECT sys,sum(*) as total FROM appDownloadRecord GROUP BY sys;
 
]; //查询条件 user_id大于0
$options = [
    'projection' => ['_id' => 0],   //不输出_id字段
    'sort' => ['user_id'=>1]       //根据user_id字段排序 1是升序，-1是降序
    'skip' => 0,                    //page  0是第一页，2是第二页 4是第三页 所以公式为$skip = ($page-1)*$limit
    'limit' => 2                   //查10条
];
$query = new MongoDB\Driver\Query([], $options);       //查询请求
$list = $manager->executeQuery('admin.colleagues',$query)->toArray(); // 执行查询 location数据库下的collection1集合
 
echo "<pre>";
print_r($list);
exit;
 
$data = [];
foreach ($list as $k=>$document) {
    echo "<pre>";
    print_r($document);
    //$data[$k]['user_id'] = $document->user_id;
    //$data[$k]['real_name'] = $document->real_name;
}
 
//查询结果
stdClass Object
(
    [user_id] => 1
    [name] => 李刚
    [position] => test engineer
    [age] => 20
)
stdClass Object
(
    [user_id] => 3
    [name] => 陈发
    [position] => php engineer
    [age] => 20
)
stdClass Object
(
    [user_id] => 2
    [name] => 侯伟
    [position] => web engineer
    [age] => 15
    [real_name] => 侯伟
)
```