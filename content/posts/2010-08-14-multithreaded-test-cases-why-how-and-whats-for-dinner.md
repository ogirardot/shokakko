---
title: MultiThreaded Test cases – Why ? How ? and What’s for dinner ?
author: ogirardot
type: post
date: 2010-08-13T22:53:26+00:00

original_post_id:
  - 412
categories:
  - Java
tags:
  - JUnit
  - MultiThreadedTC
  - VirgoRT

---
Okay so to make a small introduction (i promise a tiny one) i'm starting to contribute into <a title="Virgo OSGi Runtime" href="http://www.eclipse.org/virgo/" target="_blank">VirgoRT</a> (ex-SpringSource DM Server) for me it's an opportunity to work with great people, learn more about OSGi and work on a real JEE application server (and more).
<!--more-->

So i started to discover the kernel and decided to improve virgo's code coverage, as adding more tests is a great way to learn about how things work. And honestly i've got to say, this is great code ! it's clean, efficient and easily understandable (loose coupling and high cohesion).

But an application server is all about multi-threaded interactions, so as usual when it comes to multi-threading tests are hard, they become irrelevant, sometimes un-predictable and of course MORE COMPLICATED than the code it's supposed to test ( :-/ ).

So with Steve Powell from VMWare we decided to look further into it, came out <a title="Testing Extensions" href="http://wiki.eclipse.org/Virgo/Community/Testing_Extensions" target="_blank">this wiki page about testing extensions in VirgoRT</a> still under discussion about how to handle these tests. And after a lot of searching and re-reading the '**Java Concurrency In Practice**' Book of <a href="http://twitter.com/BrianGoetz" target="_blank">Brian Goetz</a> (that i recommend as a reference), i found this test framework : <a href="http://www.cs.umd.edu/projects/PL/multithreadedtc/index.html" target="_blank">MultiThreadedTC</a> from the university of Maryland (that created the infamous <a href="http://findbugs.sourceforge.net/" target="_blank">FindBugs</a>).

And this is my official tool as of this day.

Let me just show you a real-life example of a Barrier, this barrier can block many threads until one signal a failure (throwing an exception to all waiting threads) or a completed execution. This barrier can be respected _ad vitam eternam_ but most of the times its users (threads) use it with a timeout that will make the waiting method return false when reached. That's this point that needs the most testing.

Here is a test case that tests precisely this functionnality :

<pre>public final class BlockingSignalTest {
    private static class SignalCompletionFailsAfterWaitingExceeded extends MultithreadedTestCase {
      BlockingSignal signal;

       @Override public void initialize() {
         signal = new BlockingSignal();
       }

       public void threadFailing() {
         try {
           // awaiting for signal completion flag
           waitForTick(1);
           freezeClock();
           boolean returnSignal =
               signal.awaitCompletion(1, TimeUnit.MICROSECONDS);
           assertTick(1);
           unfreezeClock();
           Assert.assertFalse(returnSignal);
         } catch (FailureSignalledException e) {
           // this code should not be reached
           Assert.fail();
         }
       }

      public void threadSignalingCompletion() {
         waitForTick(2);
         signal.signalSuccessfulCompletion();
       }
    }

   @Test
  public void signalCompletionFailsAfterWaitingExceeded() throws Throwable {
    TestFramework.runOnce(new SignalCompletionFailsAfterWaitingExceeded());
  }
}
</pre>

You can see here without any more explainations that each thread is created as a method of the TestCase class, the test framework is giving you access to an inner clock that will help you synchronize your threads. Therefor the thread that will signal the completion will only be signalling it at Tick 2 and the first thread is executing all of its job during Tick 1.

The part that needs maybe the most explaination is the _freeze/unfreezeClock_, well Tick(s) are incremented by the framework only when all threads are waiting, but the _awaitCompletion_ triggers a _wait_ method and we don't want any Tick to be incremented during this period where the only thing we're testing is the timeout.

Therefor we can _freeze_ the clock incrementing all ticks to test our waiting methods.  
Okay, now just to compare here is the version i submitted some times ago without this test framework and with a special Thread object that could handle catching exceptions and re-throwing (in order to handle all the AssertionErrors thrown by JUnit) :

<pre>public final class BlockingSignalTests {
  @Test
  public void signalCompletionFailsAfterWaitingExceeded() throws Throwable {
    final BlockingSignal signal = new BlockingSignal();
    ExceptionCatcherThread testThread = new ExceptionCatcherThread(new Runnable(){
      public void run() {
        try {
          // awaiting for signal completion flag
          boolean returnSignal =
                    signal.awaitCompletion(1, TimeUnit.MILLISECONDS);
          assertFalse(returnSignal);
        } catch (FailureSignalledException e) {
          // this code should not be reached
          fail();
        }
     }
    });

    testThread.start();
    Thread.sleep(100);
    signal.signalSuccessfulCompletion();
    testThread.join(10);
    assertFalse("Test thread still alive after delay.", testThread.isAlive());
    testThread.rethrowUncaughtExceptions();
  }

  /**
  * Special thread designed to record uncaught exceptions
  * and re-throw the first of them on demand.
  */
  private class ExceptionCatcherThread extends Thread {
    private final Vector uncaughtExceptions = new Vector();

    public ExceptionCatcherThread(Runnable r) {
      super(r);
       this.setUncaughtExceptionHandler(new UncaughtExceptionHandler() {
         public void uncaughtException(Thread t, Throwable e) {
           uncaughtExceptions.add(e);
         }
       });
    }

    public void rethrowUncaughtExceptions() throws Throwable {
      if (!uncaughtExceptions.isEmpty())
        throw uncaughtExceptions.firstElement();
    }
  }
}
</pre>

This is clearly less understandable when you need to &#8220;take a look&#8221; and the _Thread.sleep()_ methods are definitely a source of error and false negative tests.

I can't say how much this framework seems profitable for me, so i'll let you judge on your own and according to your needs.

Hope you enjoyed it