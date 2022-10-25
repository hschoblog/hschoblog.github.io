---
layout: post
title: First Test Post
tags: [test1, test2]
author: Hyunsoo
---

Hi this is the first-ever page to post my own blog.

## Table of content

<!-- To be placed at the beginning of the post, it is where the table of content will be generated -->
* Hyunsoo
  Cho
* 
You need to put this at the beginning of the page where you want the table of content to be displayed

```html
* Hyunsoo
Cho
```

It will then render the markdown and html titles (lines that begins with `#` or using the `<h1></h1>` tages)



Tables have also been extended from Markdown:

| First Header | Second Header |
|--------------|---------------|
| Content Cell | Content Cell  |
| Content Cell | Content Cell  |

Here's an example of an image, which is included using Markdown:

![Image of a glass on a book]({{ "/assets/img/pexels/test.jpg" | relative_url }})

### Coding

{% highlight js %}
// count to ten
for (var i = 1; i <= 10; i++) {
    console.log(i);
}

// count to twenty
var j = 0;
while (j < 20) {
    j++;
    console.log(j);
}
{% endhighlight %}

### Coding2


{% highlight python %}
// print 10
for i in range(0, 11):
  print(i)


// remove
list A = [item for item in listB if item == 1]
{% endhighlight %}

### Math

Type on Strap uses KaTeX to display maths. Equations such as $$S_n = a \times \frac{1-r^n}{1-r}$$ can be displayed inline.

Alternatively, they can be shown on a new line:

$$ f(x) = \int \frac{2x^2+4x+6}{x-2} $$

