---
title: Snow leopard and Qt/PyQt 4.8.x won’t work
author: ogirardot
type: post
date: 2012-01-17T21:52:19+00:00

original_post_id:
  - 866
categories:
  - OSS
  - Python

---
<!--more-->
<p style="text-align:justify;">
  If you try to install, even with Homebrew the latest version of Qt the 4.8.x, you may end up haing a surprise like that :
</p>

<pre style="text-align:justify;">ImportError: dlopen(/usr/local/lib/python/PyQt4/QtWebKit.so, 2): Symbol not found: _kCFWebServicesProviderDefaultDisplayNameKey
  Referenced from: /Library/Frameworks/QtWebKit.framework/Versions/4/QtWebKit
  Expected in: /System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation</pre>

<p style="text-align:justify;">
  This is coming precisely from a <a href="https://bugreports.qt.nokia.com/browse/QTBUG-23157">Qt issue</a> that don't seem to be resolved anytime soon, so anyway you're warned now, revert back to 4.7.x you have no choice. Except if you want to buy Lion 🙂
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>