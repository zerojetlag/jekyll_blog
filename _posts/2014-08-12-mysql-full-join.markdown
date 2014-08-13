---
layout: post
title:  "Mysql不支持full join"
date:   2014-08-12 22:26:37
categories: sql
---

mysql不支持full join，wtf
{% highlight ruby %}
SELECT * FROM A FULL JOIN B ON A.id = B.a_id;
{% endhighlight %}

解决方法:
{% highlight ruby %}
SELECT * FROM A LEFT JOIN B ON A.id = B.a_id
UNION
SELECT * FROM A RIGHT JOIN B ON A.id = B.a_id;
{% endhighlight %}

UNION不包含重复
UNION ALL包含重复



[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
