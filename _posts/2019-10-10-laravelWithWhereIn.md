---
title: Laravel中WhereIn与多查询
date: 2019-10-10 23:40:49
---
# Laravel中WhereIn与多查询

$array_1_11 = VenousThrombusAnswer::
where(['aa'=>'111','class'=>'M4'])
->whereIn('id',['47','48','49','50','51','52','53','54','55','56','57'])
->get(['answer'])
->toArray();

 


1.ORM操作需要创建对应的model

        class User extends Eloquent

 



2.有两种方式使用数据操作对象

           a. 使用new关键字创建对象后执行对象的方法
    
           b. 直接调用static方法(实际并发静态方法，而是fascade生成的)

 



3.常用数据操作

       a.  User::find(1)    查找单条数据
       b.  User::all()        查找所有数据
       c.   User::find(1)->delete()    删除单条数据
       d.    User::destory(array(1,2,3))    删除单条或多条数据
       e.    User::save()        保存数据
        f.    User::first()        取第一条数据
       g.    Album::where('artist', '=', 'Matt Nathanson') ->update(array('artist' =>'Dayle Rees'));    指定查询条件，更新数据
        h.  User::truncate()    清空数据表，危险操作
        i.   Album::where('artist', '=', 'Something Corporate')->get(array('id','title'));    配合查询条件获取多条数据
       j. Album::pluck('artist');            返回表中该字段的第一条记录
       k. Album::lists('artist');                返回一列数据
       l.   Album::where('artist', '=', 'Something Corporate')->toSql();     获取查询的sql语句,仅用于条件，不能用户带get()之类的带查询结果的查询中

注：直接使用return 查询结果为json格式的数据

       这里使用的User为model名称

 



条件查询：

   1. 最普通的条件查询 User::where('字段名','查询字符','限制条件')     例：Album::where('title', 'LIKE', '...%')

   2. 多条件查询，使用多个where    Album::where('title', 'LIKE', '...%')->where('artist', '=', 'Say Anything')->get();

   3. 或查询操作使用orWhere(),使用方法通where

    4.直接用sql语句写查询条件    Album::whereRaw('artist = ? and title LIKE ?', array('Say Anything', '...%'))
    
    5. 其他查询方法
    
       whereIn()，whereBetween()，whereNested()子查询，orWhereNested()，whereNotIn()，whereNull(),whereNotNull()
    
    6. 快捷方式  whereUsername('king')  查询'username' = 'king'的数据，默认系统无此方法，username为字段名称

 



结果排序：

  使用order关键字：

     Album::where('artist', '=', 'Matt Nathanson')->orderBy('year')->get();   默认asc
    orderBy('year', 'desc')

 



限制结果数量

    take()方法
    
    Album::take(2)->get();                          //select * from `albums` limit 2

 



指定偏移

    Album::take(2)->skip(2)->get();        //select * from `albums` limit 2 offset 2

 

