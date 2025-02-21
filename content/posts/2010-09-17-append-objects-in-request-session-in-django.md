---
title: '[Django] Append objects in request.session'
author: ogirardot
type: post
date: 2010-09-17T10:00:47+00:00

original_post_id:
  - 541
categories:
  - Python
tags:
  - append
  - django
  - list
  - objects
  - request
  - session

---
This article is once again more of a reminder to me, i hope it will help everyone at the same time.
<!--more-->
I was experiencing some issue using <a title="Django session ref doc" href="http://docs.djangoproject.com/en/dev/topics/http/sessions/" target="_blank">Django Session</a> objects lately, i wanted to save a list in my session object and something strange happened.

The first creation of the list went just fine, but when i tried to append objects to this session everything **seemed to look fine** in the method where i was appending the data, but when from another method i tried to loop on this list, i only found the first item.

Let me show you some log to illustrate :

<pre># first method before append - state of the session object :
[('saved', ['obj1']), ('_auth_user_backend', 'django.contrib.auth.backends.ModelBackend'), ('_auth_user_id', 1)]

# first method after append:
[('saved', ['obj1', 'obj2']), ('_auth_user_backend', 'django.contrib.auth.backends.ModelBackend'), ('_auth_user_id', 1)]

# second method :
[('saved', ['obj1']), ('_auth_user_backend', 'django.contrib.auth.backends.ModelBackend'), ('_auth_user_id', 1)]</pre>

Ok now this is strange... because it looks just like it worked, and then... no.

The answer came to me (as usual) googling the question and finding the answer on the <a title="DjangoFoo tips 57" href="http://www.djangofoo.com/57/session-arraylist-append-does-not-work" target="_blank">DjangoFoo</a> website. Therefore instead of doing something like that when appending a list in a session object :

<pre>if not 'saved' in request.session or not request.session['saved']:
		request.session['saved'] = [obj]
	else:
		request.session['saved'].append(obj)</pre>

What you need to do is :

<pre>if not 'saved' in request.session or not request.session['saved']:
		request.session['saved'] = [obj]
	else:
		saved_list = request.session['saved']
		saved_list.append(obj)
		request.session['saved'] = saved_list</pre>

It's not very Pythonic or elegant, but that's the way. This limitation comes from the following advice given out by Django's documentation :

> Use normal Python strings as dictionary keys on <tt>request.session</tt>. This is more of a convention than a hard-and-fast rule

Thank you for your time,  
_Vale_