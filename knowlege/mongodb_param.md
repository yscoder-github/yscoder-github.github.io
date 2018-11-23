本文主要介绍Mongodb中CRUD常用操作、存储引擎、数据模型，以及如何使用java Driver。



一、BSON

  Mongodb中数据存储格式为BSON，和JSON非常类似，可以说在整体的结构几乎一样，只不过BSON定义了更多的数据类型，这对面向对象编程语言非常友好。
``` json
{
  "_id": {
      "$oid": "55f6c87fdefdd10de72fc024"
  },
  "name": "zhangsan",
  "age": 30,
  "is_alive": true,
  "addresses": [
      "beijing",
      "shanghai"
  ],
  "created": {
      "$date": 1442236543707
  }
}
```
