scala-akka-monitoring
=====================

Experiments with monitoring of Akka actors


= Motivation =

Akka, the great actors framework, does not have monitoring out-of-the box.
I have played with TypeSafe Console for some time - really, an admirable stuff, beautiful and handfull for deep data analysis. But it is not applicable for projects of small or medium size we are working now - it is very expensive and, most important, we do not want the monitoring be so deep!
After years of Java I can say that in most cases you need some simple tools for lightweight monitoring - to see how many threads are working, how many messages are in queues etc.
Even better if you have a simple open-source tool which you can adapt to your needs.


= How it works =

If you are familiar with profilers there are 2 concepts: sampling and instrumentation. Typesafe Console does instrumentation - it "patches" some akka classes via AspectJ to intercept some calls and gather info for further analysis.
My tool utilizes sampling - it periodically traverses the whole actor tree and all dispatchers having a reference to ActorSystem. This reference is all that it needs to work.

Access to actor tree is rather simple, excepting that the tool must be inside "akka.actor.package" (otherwise you can not compile it). Going through dispatchers was only possible Java reflection because of private fields.

I belive such monitoring is better to be added to Akka itself, without hacks and reflection - one day maybe ;)


= Examples of usage =

Look into Test.scala


= What you can not with this tool =

Time-related measurements - ex. average message procesing/wait time. This is where Typesafe Console shines!


= Further ideas =

1) JMX control - start/stop via JMX, change settings via JMX

2) publish monitoring data via JMX - not sure whether it is good or bad...

3) filter (inclusion/exclusion) for actors

4) support for several ActorSystems at once

5) play with AspectJ to implement something similar to Typesafe Console but without GUI
