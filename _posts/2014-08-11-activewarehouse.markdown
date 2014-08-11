---
layout: post
title:  "Activewarehouse使用记录"
date:   2014-08-11 22:26:37
categories: ruby
---

[activewarehouse](https://github.com/activewarehouse/activewarehouse)是个building data warehouse的gem，详情还是看github，由于这货没有文档，我写一下自己的使用经历吧

[activewarehouse-etl](https://github.com/activewarehouse/activewarehouse-etl)用于etl的过程，这个gem到时有文档，按着文档弄一般都没什么问题，就是join表的时候有点坑，join的第一个表默认用了inner join，有些时候确确实实需要left join一个表

*举例：*

{% highlight ruby %}
class DateDimension < ActiveWarehouse::Dimension
  set_order :sql_date_stamp
  define_hierarchy :custom_day, [:date]
end
{% endhighlight %}
定义date dimension，这个比较方便，lib里面就已经设计好字段了，只需要自己定义一下hierarchy，可定义多个，需要年月日层次的话可以用：
{% highlight ruby %}
define_hierarchy :custom_hierarchy, [:calendar_year, :calendar_month_name, :date]
{% endhighlight %}

再定义一个article dimension，假设article这个纬度有个category的字段用于观察article visit的数据
{% highlight ruby %}
class ArticleDimension < ActiveWarehouse::Dimension
  define_hierarchy :category, [:category]
end
{% endhighlight %}

定义article visit fact
{% highlight ruby %}
class ArticleVisitFact < ActiveWarehouse::Fact
  aggregate :visits, :type => :count,  :label => "count"

  dimension :date
  dimension :article
end
{% endhighlight %}

定义article cube(activewarehouse用来整理数据)
{% highlight ruby %}
class ArticleVisitCube < ActiveWarehouse::Cube
  reports_on :article_visit #关联的事实表
  pivots_on :date, :article #关联的纬度
end
{% endhighlight %}

上述所做的一切是为了得到：
![table]({{ site.baseurl }}/assets/table.png)

*未完待续...*

[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
