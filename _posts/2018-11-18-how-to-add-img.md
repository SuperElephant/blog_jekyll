---
layout: post
title: How to add image in Jekyll
tags: [eng, note, it]
excerpt_separator: use
---

To insert image in the blog we can simply use

{% highlight ruby %}
![AltText](/path/to/img.jpg)
{% endhighlight %}

However, if we need to adjust the size of the picture, you can use html tag
{% highlight ruby %}
<img src="{{site.baseurl}}/assets/img/test.jpg" width="50%" alt="AltText" />
{% endhighlight %}

Also, you can use the mix style:
{% highlight ruby%}
![AltText]({{site.baseurl}}/assets/img/test.jpg){:width="70%"}
{% endhighlight %}
