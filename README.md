# akka-blocking-example

This sample project demonstrates how putting blocking operations,
even if they are fundamentally not blocking, on a dispatcher thread
causes Akka to lock up.

Simply run `main()` on `AkkaQuickstart` and produce a thread dump to
show a call stack being blocked somewhere that doesn't make sense.

e.g.

```
"helloakka-akka.actor.default-dispatcher-6@2241" prio=5 tid=0x1c nid=NA waiting
  java.lang.Thread.State: WAITING
	  at jdk.internal.misc.Unsafe.park(Unsafe.java:-1)
	  at java.util.concurrent.locks.LockSupport.park(LockSupport.java:194)
	  at java.util.concurrent.FutureTask.awaitDone(FutureTask.java:447)
	  at java.util.concurrent.FutureTask.get(FutureTask.java:190)
	  at com.example.Greeter.onGreet(Greeter.java:82)
	  at com.example.Greeter$$Lambda$227.1237652720.apply(Unknown Source:-1)
	  at akka.actor.typed.javadsl.BuiltReceive.receive(ReceiveBuilder.scala:213)
	  at akka.actor.typed.javadsl.BuiltReceive.receiveMessage(ReceiveBuilder.scala:204)
	  at akka.actor.typed.javadsl.Receive.receive(Receive.scala:53)
	  at akka.actor.typed.javadsl.AbstractBehavior.receive(AbstractBehavior.scala:64)
	  at akka.actor.typed.Behavior$.interpret(Behavior.scala:274)
	  at akka.actor.typed.Behavior$.interpretMessage(Behavior.scala:230)
	  at akka.actor.typed.internal.adapter.ActorAdapter.handleMessage(ActorAdapter.scala:129)
	  at akka.actor.typed.internal.adapter.ActorAdapter.aroundReceive(ActorAdapter.scala:106)
	  at akka.actor.ActorCell.receiveMessage(ActorCell.scala:577)
	  at akka.actor.ActorCell.invoke(ActorCell.scala:547)
	  at akka.dispatch.Mailbox.processMailbox(Mailbox.scala:270)
	  at akka.dispatch.Mailbox.run(Mailbox.scala:231)
	  at akka.dispatch.Mailbox.exec(Mailbox.scala:243)
	  at java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:290)
	  at java.util.concurrent.ForkJoinPool$WorkQueue.topLevelExec(ForkJoinPool.java:1020)
	  at java.util.concurrent.ForkJoinPool.scan(ForkJoinPool.java:1656)
	  at java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1594)
	  at java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:177)
```

see https://doc.akka.io/docs/akka/current/typed/dispatchers.html#blocking-needs-careful-management for more details.