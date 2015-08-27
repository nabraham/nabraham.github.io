---
layout: post
title: "50 Eulers in Scala"
description: "yet another project euler repo"
category: 
tags: [scala, euler]
---
Under the umbrella task of learning Scala, I decided to try a few [Project Euler](https://projecteuler.net/archives) problems.  A few turned in to many, and now that I've reached the end of page 1, it's time to take a break and go back and finish [the book](http://www.amazon.com/Programming-Scala-Comprehensive-Step-Step/dp/0981531644).

I definitely enjoyed it; I remain unconvinced that it is the right choice professionally:

1.  If you do not plan on maintaining your code, it will likely suffer a quick death when you hand it off.  Someone who knows Scala and thinks functionally is not likely to be in the business of maintaining someone else's code.
2.  I am not a fan of the syntactic sugar options.  It's not that much extra work to write <code>x => x + 1</code> as opposed to <code>_ + 1</code>.  Similar gripes for the spaces vs. dots.  And leaving types off functions.  Just pick one style, make everyone learn it, and collaboration will be much easier. 

On the other hand, here were some of my favorite dense solutions.  If you want denser, go check out [Pavel Fatin's blog](https://pavelfatin.com/scala-for-project-euler/).  My goal was to leave the code in such a state that non-Scala person see pretty easily what's going on.  Towards the end, I just wanted to finish.  The full repo is [here](https://github.com/nabraham/pe-scala.git).

**Dealing with large numbers**
{% highlight scala %}
//  The series, 1^1 + 2^2 + 3^3 + ... + 10^10 = 10405071317.
//
//  Find the last ten digits of the series, 1^1 + 2^2 + 3^3 + ... + 1000^1000.
def p048(): Unit = {
    assert((1 to 1000).map(x => BigInt(x).pow(x))
        .sum.toString.takeRight(10) == solutions("48"))
}
{% endhighlight %}

**Permutations**
{% highlight scala %}
//  We shall say that an n-digit number is pandigital if it makes use of all the digits 
//  1 to n exactly once. For example, 2143 is a 4-digit pandigital and is also prime.
//
//  What is the largest n-digit pandigital prime that exists?
def p041(): Unit = {
    def isPrime(n: Int):Boolean = { (2 to Math.sqrt(n).toInt).forall(n % _ != 0) }
    val rez = (1 to 7).reverse.permutations.find(p => isPrime(p.mkString("").toInt))
    assert(rez.get.mkString("") == solutions("41"))
}
{% endhighlight %}
_all 8,9 digit pandigitals are divisible by 3 and therefore not prime_

**Java Interop**
{% highlight scala %}
// How many Sundays fell on the first of the month during the twentieth century 
// (1 Jan 1901 to 31 Dec 2000)?
def p019(): Unit = {
    val cal = Calendar.getInstance()
    val sdf = new SimpleDateFormat("yyyy/M/d")
    val firsts = for (year <- (1901 to 2000);
                      month <- (1 to 12)
    ) yield { 
        cal.setTime(sdf.parse("" + year + "/" +  month + "/1" )); 
        cal.get(Calendar.DAY_OF_WEEK) 
    }

    assert(firsts.count(_ == 1) == solutions("19").toInt)
}
{% endhighlight %}
