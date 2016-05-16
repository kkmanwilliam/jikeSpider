# 即刻App
即刻App是我很喜欢的一款产品，事实上最近两年很少看到这样令人喜爱的互联网产品，去年就看到两个，一个是Surge，一个就是即刻。

喜欢即刻的原因是它能帮我解决了一些信息获取上的问题，一些没有专门组织生产和维护的消息，特别是一些小众、冷门信息，比如BBC新出了什么好的纪录片，比如SpaceX有什么新闻，等等……即刻能通过爬虫和人工筛选编辑的方式第一时间获取这些消息，显示在我的timeline里面。

另外一个好处是，用即刻能很方便地追踪一个事件、新闻的动态发展，跟这个事件相关的信息流都会展示在这个事件主题里，所以可以很清晰的看到整个事件的发展过程。

因为想知道即刻App上被订阅最多的主题是哪些，都有什么特点，所以我写了一个爬虫来爬取主题数据。

# 爬虫思路

1. 预先加载[最近三个月关注top20](https://github.com/gentleming/jikeSpider/blob/master/jiketop20.txt)的主题到线程队列

2. 多线程爬取队列中的20个主题页面，获取每个主题页面的3个关联主题，爬取它们的标题、关注人数、url，放入线程队列，并存入数据库。

3. 如此反复，对不断加入队列的url进行爬取，获取更多的关联主题，如果和数据库中url不同，则放入数据库。

# 运行结果

由于这种关联搜索的效率比较低，爬虫运行了几个小时，最后获取了[998条主题数据](https://github.com/gentleming/jikeSpider/blob/master/jike.csv)，不过这个样本已经足够了。

# 结果分析
1. 超过10万关注量的主题只有25个，这个25个主题的关注量占全部998个主题关注量的60%。#马太效应，关注量越多，越容易获得更多关注量

2. 关注量前200的主题，获得的关注量是78%。#非常贴近二八定律，有意思

3. 主题各式各样，内容涵盖方方面面，五花八门，有些很难给出确切的分类。不过从这些主题来看，可以判别的是即刻App用户的普遍素质是很高的，对歪国的科技和文化了解很多，重度的互联网用户，苹果用户居多。另外，看美剧明显多于看韩剧的，这也说明男性用户比例占大部分。所以我猜有大量的用户是程序员或产品经理吧，至少也是互联网从业者。

4. 一定要总结点规律的话，我觉得可以把主题分为两类：**更新提醒类**、**精选推荐类**，前者很适合用爬虫自动抓取，后者可能需要人工运营干预。

5. 在998个主题中，
  用「新」搜索得到452个结果，用「新的」搜索有105个结果，用「提醒」搜索有105个，用「精选」搜索有78个，用「推荐」搜索有69个，用「每日」搜索有65个。
 我觉得这个结果很清晰地反映即刻用户的心理状态，害怕错过信息，比如有80多万人关注「一觉醒来世界发生了什么」和「今天微博都在热议什么」，深深的信息焦虑症啊。
另外一方面用户又希望有高效的信息获取渠道，比如通过人工精选推荐，能有效地做好信息过滤，减少在垃圾信息上浪费宝贵时间。

详细爬虫信息请访问我的[Github](https://github.com/gentleming/jikeSpider)