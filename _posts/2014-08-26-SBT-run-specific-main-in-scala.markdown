---
layout: post
title:  "Refresher on SBT - Part 1"
date:   2014-08-26 23:00:18
categories: ['SBT']
---
I am currently going through a refresher with SBT. I don't use it as the main build system on my projects so I keep forgetting the details. There is a bunch of convention, and in some cases, magic, that surrounds SBT which frankly you can't remember till you use it day-in-day-out. This post chronicles some gotchas I discovered.

# Conventions all the way down
The convention starts out really early with an SBT based project. When you run `sbt` for instance, it actually runs an interactive shell. When you run `sbt compile` the compile task will look for code assets to compile in designated locations. For instance, `<project>/src/main/scala` or `project/src/main/java`. When you type `sbt run` it will try to determine entry points to the application and if only one is discovered automatically run it.

If for instance you have multiple main entry points in the project, e.g.:
{% highlight scala %}
object Test {
  def main(args: Array[String]) = println("Hello world.")
}
{% endhighlight %}
{% highlight scala %}
object Test2 extends scala.App {
  val argstr = args mkString ","
  println(s"Running scala.App with args: $argstr")
}
{% endhighlight %}

Running `sbt run` on the command line will result in:
{% highlight bash %}
sbt run
[info] Loading project definition from /Users/vivekl/IdeaProjects/Test/project
[info] Set current project to Test (in build file:/Users/vivekl/IdeaProjects/Test/)

Multiple main classes detected, select one to run:

 [1] Test
 [2] Test2
{% endhighlight %}

To run a specific main and to optionally pass arguments, run `sbt "run-main Test2 arg1 arg2..."`

e.g.: `sbt "run-main Test2 you gotta know how to quote 'em"`
{% highlight bash %}
[info] Running Test2 you gotta know how to quote 'em
Running scala.App with args: you,gotta,know,how,to,quote,'em`
{% endhighlight %}

*Notice the quoting for the sbt command above!* Not quotting as above will cause you problems.
