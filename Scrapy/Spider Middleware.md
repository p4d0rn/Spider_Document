# Spider Middleware

Response：
Downloader -> Engine --Spider Middleware--> Spider

Item：
Spider --Spider Middleware--> Engine  --> Item Pipeline

Request：
Spider --Spider Middleware--> Engine  --> Scheduler

同样，Scrapy提供了很多内置的Spider Middleware，定义在`SPIDER_MIDDLEWARES_BASE`

> SPIDER_MIDDLEWARES_BASE = {
>
> ​    'scrapy.spidermiddlewares.httperror.HttpErrorMiddleware': 50,
> ​    'scrapy.spidermiddlewares.offsite.OffsiteMiddleware': 500,
> ​    'scrapy.spidermiddlewares.referer.RefererMiddleware': 700,
> ​    'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware': 800,
> ​    'scrapy.spidermiddlewares.depth.DepthMiddleware': 900,
>
> }

自定义的Spider Middleware需要在settings.py的SPIDER_MIDDLEWARES中注册
键值表示中间件的优先级，数字越小越靠近Engine

Spider Middleware可以通过下面方法拓展功能

* process_spider_input(response, spider)

处理传给Spider的Response对象

返回None或抛出异常

* process_spider_output(response, result, spider)

当Spider处理Response后，返回结果，该方法被调用

必须返回Request或Item对象的可迭代对象

* process_spider_exception(response, exception, spider)
* process_start_requests(start_requests, spider)

以Spider启动的Request为参数被调用



HttpErrorMiddleware过滤除状态码除2xx以外的Response

OffsiteMiddleware过滤不符合allowed_domains的Request

