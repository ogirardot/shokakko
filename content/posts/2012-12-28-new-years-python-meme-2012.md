---
title: New Year’s Python Meme 2012
author: ogirardot
type: post
date: 2012-12-28T21:38:02+00:00

publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:2397699;i:347;}s:2:"wp";a:1:{i:0;i:0;}}'
publicize_twitter_user:
  - ogirardot
categories:
  - OSS
  - Python
tags:
  - Python meme

---
<!--more-->
### 1. What is the coolest Python application, framework or library you have discovered in 2012?

Mainly for [APPARTINFO][1], but not only, i've been using every single part of Django and this framework is still as awesome as usual. But as i must talk about what i've discovered in 2012, i have to talk about some libs that makes the Django ecosystem great that i discovered this year :

  * <span style="line-height:13px;"><a title="Django-Tastypie" href="http://django-tastypie.readthedocs.org/en/latest/">Tastypie</a> : to make wonderful standard REST API easily </span>
  * <span style="line-height:13px;"><a title="Haystack" href="http://django-haystack.readthedocs.org/en/latest/">Haystack</a> : to use any search engine (Woosh, Solr, Elastic Search, ...) and sync their indexes easily with any Django model. The &#8220;realtime&#8221; mode is really great ! especially if you design it to use a Celery/RabbitMQ queue for every update of the Solr index.</span>
  * <span style="line-height:13px;">A special note on <a title="Django Social Auth" href="http://django-social-auth.readthedocs.org/en/latest/index.html">django-social-auth</a> that i've been using more than i ever thought i would...</span>

<span style="line-height:13px;">I switched all of my django projects to <a title="Gunicorn" href="http://gunicorn.org/">Gunicorn</a> + <a title="Nginx" href="http://wiki.nginx.org/Main">Nginx</a>, linking the two of them with unix sockets, instead of Apache/Nginx, and i'm never ever ever coming back that's for sure.</span>

[IPython][2] and its notebook is also a great achievement of this year

But the coolest ever this year has been the [Pandas][3] lib with its wonderful [DataFrame][4] structure, just like [scikit-learn][5], [numpy][6] and [nltk][7], i'm dying to use them more but i lack the scientific background, time, and skills to make the best of these tools.

### 2. What new programming technique did you learn in 2012?

I've been doing more functional python, but mainly using more list comprehension, more efficiently... It's probably coming from my increasing Scala usage 😉

### 3. What is the name of the open source project you contributed the most in 2012? What did you do?

I've been working a lot on private/closed source products, but my biggest open source contributions were on :

  * <span style="line-height:13px;"><a title="LT-Agora" href="https://github.com/LateralThoughts/lt-agora">LT-Agora</a> : A collective decision making tool create to handle democracy within <a href="http://www.lateral-thoughts.com/">LateralThoughts</a>. I created the project and i am getting it out to the world.<br /> </span>
  * [NNMon][8] : the net neutrality project http://respectmynet.eu/ created by &#8220;La Quadrature du net&#8221; where i cleaned things up a bit and did my part.

### 4. What was the Python blog or website you read the most in 2012?

[Wes McKinney's blog][9] was number one this year, i already said it, but i LOVE the Pandas project. But i read also a lot [Alex Gaynor's blog][10] and the [PyPy Status Blog][11].

### 5. What are the three top things you want to learn in 2013?

I want to do more Machine Learning and Natural Language Processing and leverage the existing tools in python or create new one to create great tools.

### 6. What are the top software, app or lib you wish someone would write in 2013?

I would love a Python Conditional Random Fields or semiCRF lib to train a model that i can understand and use in confidence.

What about you ? What are you looking for ?

_Vale_

 [1]: http://www.appartinfo.com "APPARTINFO"
 [2]: http://ipython.org/ "IPython"
 [3]: http://pandas.pydata.org/ "Pandas lib"
 [4]: http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe "Pandas' DataFrame"
 [5]: http://scikit-learn.org/stable/ "Scikit learn"
 [6]: http://www.numpy.org/ "Numpy"
 [7]: http://nltk.org/
 [8]: https://github.com/stef/nnmon "NNMON"
 [9]: http://wesmckinney.com/ "Pandas creator"
 [10]: http://alexgaynor.net/
 [11]: http://morepypy.blogspot.fr/