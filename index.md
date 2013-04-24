---
layout: default
---


Lightweight Modular Staging (LMS) is a runtime code generation
approach. The framework provides a library of core components for
building high performance code generators and embedded compilers in
Scala.

Two closely related projects are [Delite](http://stanford-ppl.github.com/Delite/), 
a framework for domain specific languages (DSLs) that generates parallel 
code for heterogeneous devices, and [Scala Virtualized](https://github.com/tiarkrompf/scala-virtualized/wiki), 
a set of minimal extensions to the Scala compiler that make embedding 
DSLs more seamless.


## Abstraction Without Regret

Turn nice high-level programs into fast low-level code. Strip abstraction overhead from generic programs. Add domain-specific optimizations.

{% highlight scala %}
class Vector[T:Numeric:Manifest](val data: Rep[Array[T]]) {
  def foreach(f: Rep[T] => Rep[Unit]): Rep[Unit] =
    for (i <- 0 until data.length) f(data(i))
  def sumIf(f: Rep[T] => Rep[Unit]) = { 
    var n = zero[T]; foreach(x => if (f(x) n += x)); n }
}

val v: Vector[Double] = ...
println(v.sumIf(_ > 0))
{% endhighlight %}

<!-- TODO: use grid-based css style file -->

<table style="border: 0px;">
<tr>
<td markdown="1" style="border: 0px; padding-left: 0px;">
{% highlight scala %}
var n: Double = 0.0
var i: Int = 0
val end = data.length
while (i < end) {
  val x = data(i)
  val c = x > 0
  if (c) n += x
}
println(n)
{% endhighlight %}

</td>
<td markdown="0" style="border:0px;padding-left:50px;">

Running the snippet above will generate the program on the left: All functions are inlined, 
all generics specialized, all type class instances removed.
All of this is guaranteed by the type system: Expressions of type `Rep\[T\]` become
part of the generated program, everything else is evaluated when the original
snippet is run. 

</td>
</tr>
</table>

Actually this is only half of the story: The output on the left is not produced
directly, but going through an extensible intermediate representation (IR) that
can be further transformed and optimized.
[Read more...](#docs)


## Run Scala Anywhere

Generate code for heterogenenous targets. Compile Scala to JavaScript, SQL, CUDA, C...

* CUDA, OpenCL, C: [Delite](http://stanford-ppl.github.com/Delite/)
* JavaScript: [JS-SCALA](https://github.com/js-scala/js-scala)
* SQL: [SIQ](http://code.google.com/p/scala-integrated-query/)
* VHDL (work in progress)


## Getting Started

Clone the [GitHub repo](http://github.com/TiarkRompf/virtualization-lms-core). It contains an extensive test suite with examples.

    git clone git://github.com/TiarkRompf/virtualization-lms-core.git

Be sure to check out the [docs](#docs) below!

## Background / Documentation {#docs}

Overview presentations:

* [Intro to LMS](https://docs.google.com/presentation/pub?id=1-wMnoDrIBTF1qDbhXtHVxOmCE4oP2SzaDk8o-2Ef3Bo&start=false&loop=false&delayms=3000) by Julien Richard-Foy
* [Building Blocks for Performance-Related DSLs](http://ppl.stanford.edu/papers/dsl11-rompf-slides.pdf) by Tiark Rompf

Talks/videos from ScalaDays, Spring 2012:

* [Scala-Virtualized, LMS and Delite](http://skillsmatter.com/podcast/scala/high-level-high-performance-programming-with-scala-virtualized-lms-and-delite) by Tiark Rompf
* [High-Performance DSLs with Delite](http://skillsmatter.com/podcast/scala/high-performance-dsl-delite) by Arvind Sujeeth
* [JavaScript as an Embedded DSL](http://skillsmatter.com/podcast/agile-testing/javascript-embedded-dsl-scala) by Nada Amin


Tutorial write-ups:

* [LMS Tutorial](https://github.com/julienrf/lms-tutorial/wiki) by Julien Richard Foy
* [OptiML getting started guide](http://stanford-ppl.github.com/Delite/optiml/getting_started.html) by Arvind Sujeeth (about Delite and OptiML)
* [Understanding Rep](tutorial1.html) by Sandro Stucki (work in progress)

Comprehensive, but many many pages: 

* Tiark's [thesis](http://lampwww.epfl.ch/~rompf/thesis_120716.pdf)

Training courses, full-day tutorials:

* [Delite Tutorial](http://cgo2012.hyperdsls.org) at CGO, Spring 2012
* [Stanford CS442](http://www.stanford.edu/class/cs442/) Spring 2011


Conference and Journal publications:

* [POPL'13](http://ppl.stanford.edu/papers/popl13_rompf.pdf), 
  [CACM 2012](): LMS and extensible compilers
* [PACT'11](), [IEEE Micro 2011](): Delite
* [ECOOP'12](): JavaScript
* [PEPM'12](): Scala-Virtualized
* [more](publications.html)


Related projects:
* [Delite](http://stanford-ppl.github.com/Delite/)
* [OptiML](http://stanford-ppl.github.com/Delite/optiml/index.html)
* [Scala Virtualized](https://github.com/tiarkrompf/scala-virtualized/wiki)
* [JS-Scala](https://github.com/js-scala/js-scala)

<!-- http://www.infoq.com/interviews/amin-scala -->