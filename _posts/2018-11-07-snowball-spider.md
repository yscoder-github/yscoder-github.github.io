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
![avatar](/img/xueqiu_feed_spider/pic_of_feed.png)

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
![avatar](/img/xueqiu_feed_spider/request.png)

 [feed流请求地址](https://xueqiu.com/v4/statuses/public_timeline_by_category.json?since_id=-1&max_id=-1&count=10&category=104)

直接抓取上述地址会返回403错误，需要带上Cookies才可以正确的抓取,如果直接将上述xml请求地址投喂给Scrapy的话会导致异常。所以需要先访问雪球的首页来获取Cookies
``` python
_category = {
    '-1': "头条",
    '6': "直播",
    '105': "沪深",
    '111': "房产",
    '102': "港股",
    '104': "基金",
    '101': "美股",
    '113': "私募",
    '114': "机车",
    '110': "保险"
}
_topic_url = \
    "https://xueqiu.com/v4/statuses/public_timeline_by_category.json?since_id=-1&max_id={}&count=20&category={}"

_host_url = 'https://xueqiu.com/'
start_urls = (
   _host_url,
)
def parse(self, response):
    for code in self._category:
        topic_url = self._topic_url.format(-1, code)
        yield Request(topic_url,
                      meta={'code': code},
                      callback=self.parse_request)

                      def parse_request(self, response):
                          try:
                              json_data = json.loads(response.text)
                              next_max_id = json_data.get('next_max_id', -1)
                              topic_list = json_data.get('list', [])
                              code = response.meta['code']
                              print code
                              if next_max_id == -1 or topic_list == []:
                                  return
                              for topic in topic_list:
                                  topic_item = TopicBriefItem()
                                  data = json.loads(topic.get('data'))
                                  topic_brief_url = urljoin(self._host_url, data.get('target'))
                                  topic_item['feedback'] = data.get('feedback')
                                  topic_item['pic'] = data.get('pic')
                                  topic_item['reply_count'] = data.get('reply_count')
                                  topic_item['id'] = data.get('id')
                                  topic_item['topic_pic'] = data.get('topic_pic')
                                  topic_item['title'] = data.get('title')
                                  topic_item['first_pic'] = data.get('first_pic')
                                  topic_item['cover_pic'] = data.get('cover_pic')
                                  topic_item['source'] = data.get('source')
                                  topic_item['link_stock_desc'] = data.get('link_stock_desc')
                                  topic_item['score'] = data.get('score')
                                  topic_item['retweet_count'] = data.get('retweet_count')
                                  topic_item['topic_pic_hd'] = data.get('topic_pic_hd')
                                  topic_item['description'] = data.get('description')
                                  topic_item['reweeted_status'] = data.get('reweeted_status')
                                  topic_item['view_count'] = data.get('view_count')
                                  topic_item['quote_cards'] = data.get('quote_cards')
                                  topic_item['topic_title'] = data.get('topic_title')
                                  topic_item['user_profile'] = data.get('profile')
                                  topic_item['target'] = data.get('target')
                                  topic_item['created_at'] = data.get('created_at')
                                  topic_item['promotion'] = data.get('promotion')
                                  topic_item['tag'] = data.get('tag')
                                  topic_item['link_stock_symbol'] = data.get('link_stock_symbol')
                                  topic_item['topic_desc'] = data.get('topic_desc')
                                  yield topic_item  # 存储
                                  yield Request(topic_brief_url, callback=self.parse_response)   # 发出请求
                              yield Request(self._topic_url.format(next_max_id, code),
                                            meta={'code': code},
                                            callback=self.parse_request)
                          except:
                              raise      
```

![avatar](/img/xueqiu_feed_spider/data_sample.png)
#### response中的关键字段
* list
* addition
* tip
* next_max_id 下一条信息流的最大id
* next_id 下一条信息流的id

#####
