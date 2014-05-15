---
layout: post_page
title: ASync calls in a loop
---

One of a typical use cases I come across many times is that of making async calls in a loop. The following example shows one such scenario:

{% highlight javascript %}
for (var i = 0; i < 10; i++) {
	setTimeout(function(){
		console.log(i);
	}, 100);
}
{% endhighlight %}

It is very easy to overlook the issue with the above code. While we are expecting a serial order to be printed out, all we see in the output is the number 10. Closures come in handy here. They allow us to create a snapshot of the state and preserve it for future here. Here is how we would preserve the value of i when the callback is invoked -

{% highlight javascript %}
for (var i = 0; i < 10; i++) {
	setTimeout((function(t){
		return function(){
			console.log(t);
		}
	})(i), 100);
}
{% endhighlight %}

What we are doing here is creating a self calling function and passing it the value that we want to preserve since it will change pretty soon when the loop iterates over.
