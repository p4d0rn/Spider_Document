# Item Pipeline

Response：
Spider -> Engine -> Item Pipeline

Item Pipeline的主要功能

* 清洗HTML数据
* 检查爬取字段
* 查重丢弃重复内容
* 持久化存储

自定义Item Pipeline可以通过下面方法来扩展功能

* process_item(item, spider)

必须实现这个方法，默认使用该方法对Item进行处理，如进行数据处理或将数据写入数据库

返回Item对象或抛出DropItem异常

若返回Item对象，此Item会被低优先级的Item Pipeline处理

若抛出DropItem异常，此Item直接被抛弃

* open_spider(spider)

在Spider开启时被自动调用，可以做一些初始化操作，如开启数据库连接

* close_spider(spider)

在Spider关闭时被自动调用，可以做一些收尾操作，如关闭数据库连接

* from_crawler(cls, crawler)

是一个类方法，用@classmethod标识。通过crawler可以获取Scrapy的所有核心组件，如全局配置

参数cls就是Class，该方法返回一个Class实例

# 实战

🪁Target: https://ssr1.scrape.center/

:four_leaf_clover:Data Storage: MongoDB



