# redis-dump
最近工作中用到的，redis的全量dump和load

最近有个需求是需要将redis 导出再 导入到另一个实例，网上有个redis-dump的工具，提供了导出功能。到他是使用key*的，有点坑。。
所以自己用go写个工具也更灵活配置。



基本流程
```
 dump
   select 0-x
   scan
   type
   ttl
   get/hgetall/ ...
   to jsonEncode

 load
   jsonDecode
   check type
   set / hmset / zadd / ...
   expice
```
数据格式
```
 {"db":0,"key":"hash1","ttl":-1,"type":"hash","value":{"a":"1"},"size":2}
 {"db":0,"key":"set1","ttl":-1,"type":"set","value":["1"],"size":1}
 {"db":0,"key":"list1","ttl":-1,"type":"list","value":["1","a"],"size":2}
 {"db":0,"key":"zset1","ttl":-1,"type":"zset","value":[["1",1.0]],"size":4}
 {"db":0,"key":"string1","ttl":-1,"type":"string","value":"1","size":1}
```
