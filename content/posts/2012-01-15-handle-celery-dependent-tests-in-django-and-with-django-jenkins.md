---
title: Handle Celery-dependent tests in Django and with django-jenkins
author: ogirardot
type: post
date: 2012-01-15T16:45:33+00:00

original_post_id:
  - 861
categories:
  - OSS
  - Python

---
<p style="text-align:justify;">
  So in your life, one of these days, you're going to realize you need tests, and that &#8220;maybe&#8221; you also need to test components that depend on several Celery tasks.
</p>
<!--more-->

<p style="text-align:justify;">
  Well to help you make this day more productive and less painful, here's a few tips.
</p>

<p style="text-align:justify;">
  First to make it work with Django-celery, a pretty good but small documentation is available here<a href="http://ask.github.com/django-celery/cookbook/unit-testing.html"> http://ask.github.com/django-celery/cookbook/unit-testing.html</a> it may seems small and not enough, but it actually is enough. To sum up all you need to make it work with <em>./manage.py test </em>is to change the test runner to :
</p>

[sourcecode language=&#8221;python&#8221;]  
TEST\_RUNNER = 'djcelery.contrib.test\_runner.CeleryTestSuiteRunner'  
[/sourcecode]

<p style="text-align:justify;">
  But it's not enough, soon when you think everything's over, you'll want to deploy and make it go through Jenkins's testing processes. Well i don't know about you but to me the <a href="https://github.com/kmmbvnr/django-jenkins">django-jenkins</a> project is quite a good one and i use it everyday but it has one flaw, it already as its designated TEST_RUNNER so if you try to execute your tests through <em>./manage.py jenkins</em> it won't work and you'll get at least a <strong>Connection Refused </strong>error.
</p>

<p style="text-align:justify;">
  Here's how to fix this, the documentation says if you want to replace the TEST_RUNNER you may do so, but the class you'll use needs to inherit from <em>django_jenkins.runner.CITestSuiteRunner </em>and if you want the Celery tasks to work you need to use the <em>djcelery.contrib.test_runner.CeleryTestSuiteRunner</em>.
</p>

<p style="text-align:justify;">
  Fair enough, here's what i did. I created a new class :
</p>

[sourcecode language=&#8221;python&#8221;]  
from django_jenkins.runner import CITestSuiteRunner  
from djcelery.contrib.test_runner import CeleryTestSuiteRunner

class MixedInTestRunner(CITestSuiteRunner, CeleryTestSuiteRunner):  
pass  
[/sourcecode]

<p style="text-align:justify;">
  Why not ? and used it as such defining the new TEST_RUNNER by changing the settings's variable JENKINS_TEST_RUNNER to the newly created class.
</p>

<p style="text-align:justify;">
  And voilà.
</p>