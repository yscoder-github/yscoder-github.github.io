---
layout: post
title: "「雪球」新闻feed流爬虫!"
subtitle: ""
author: "YinShuai"
header-img: "img/stock.jpg"
header-mask: 0.3
tags:
  - 雪球
  - 股票
  - 爬虫
  - Scrapy
---
#### 雪球是一个什么样的网站？
> [雪球股票](https://xueqiu.com),聪明的投资者都在这里 - 雪球提供沪深港美股票实时行情、实战交流、实盘交易。



##### 雪球的Feed流样式
![avatar](img/xueqiu_feed_spider/pic_of_feed.png)

如上图所示为用户为从雪球首页截取出来的信息流，从图中可以看出雪球的信息流包含如下几个模块:
* 头条
* 直播
* 沪深
* 房产
* 港股
* 基金
* 美股
* 私募
* 汽车
* 保险

---  
雪球的请求地址如下：
![avatar](img/xueqiu_feed_spider/request.png)


 [feed流请求地址](https://xueqiu.com/v4/statuses/public_timeline_by_category.json?since_id=-1&max_id=-1&count=10&category=104)


![avatar](img/xueqiu_feed_spider/data_sample.png)
#### response中的关键字段
* list
* addition
* tip
* next_max_id 下一条信息流的最大id
* next_id 下一条信息流的id

##### 如何开始一个爬虫
