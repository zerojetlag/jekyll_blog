---
layout: post
title:  "Fast random"
date:   2014-09-1 10:26:37
categories: sql
---

FastRandom这个lib能为activerecord提供random的功能，其实就是拿出一个随机的id，然后比这个id大的记录拿出来，具体实现是：
{% highlight ruby %}
join (SELECT CEIL(RAND() * (SELECT MAX(id) FROM xxx)) AS gid) AS r2
{% endhighlight %}
也就是把表中最大的id拿出来，乘上一个小于1的随机数，然后，把id大于这个数的记录拿出来
{% highlight ruby %}
where (xxx.id > r2.gid)
{% endhighlight %}
正因如此，用到fast random的表id就不能任意妄为，一旦id中断，中间出现几个数量级差就要悲剧了，如id=222，然后人为添加了id是4444的记录，之后新加的记录的id就从4444开始了，然后id<4444再被random出来的概率就非常小了

[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
