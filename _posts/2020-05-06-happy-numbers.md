---
layout: post
title: Seven Steps to Happiness
author: Alexander Wood
description: The Least Unhappy Numbers
excerpt_separator: <!--more-->
image: assets/images/20200506-164.svg
image_caption: A diagram showing an unhappy number caught in a loop.
tags: leetcode happy-numbers number-theory
---


<h2>The Happy Numbers Problem</h2> 

I was playing around on Leetcode going through some random prompts when I ran across the old &ldquo;happy numbers&rdquo; problem.

<!--more-->

A <b>happy number</b> is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits. Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

For example, starting with the number $$164$$ would lead you to $$1^2 + 6 ^2 + 4 ^2 = 53$$, then to $$5^2 + 3^2 = 34$$, et cetera, until you reach a loop ending at $$89$$. 

<span class="image fit"><img src="{% link assets/images/20200506-164.svg %}" alt="89 is a happy number" /></span>

While some may say $$13$$ is an unlucky number, it is quite happy. $$109$$ is also happy, as we see below:

<span class="image fit"><img src="{% link assets/images/20200506-109.svg %}" alt="109 is not a happy number" /></span>

<h2>My First Solution</h2>

The Leetcode challenge was to write a function that returns True if a number is happy, False otherwise. Easy, right? I typed up and submitted my first solution:

{% gist 5bad1c882d878c8beea35463f3389d17 %}

This solution keeps track of all numbers seen so far in the hash table <code>mapped</code>. The first <code>while</code> loop on line 11 replaces the current number with the sum of its digits until either the sum is equal to one or the cycle begins repeating. 

Line 17 in the nested <code>while</code> loop takes the last digit of the current number, squares it, and adds it to the sum. Line 18 removes this last digit, and the loop repeats the process until all of the digits are added.

<h2>The Least Happy Numbers</h2>

What concerned me right away about this solution is the complexity. In other words, how many iterations does it take to either begin repeating a cycle, or converge to $$1$$? What is the worst-case scenario? Average? 

<span class="image fit"><img src="{% link assets/images/20200506-happy-graph.svg %}" alt="" /></span>

I counted the number of steps required for each of the first million integers to either land on $$1$$ or get caught in a cyle. It never took more than 20 steps. Happy numbers required less steps &mdash; at most 7!

Okay, so, it isn't a problem for relatively small numbers. But a million is not that big. What about large numbers? It turns out that this isn't a trivial question! 

In 1994, mathematician Richard K. Guy posed a series of questions about happy numbers in Problem E34 of his book <i>Unsolved Problems in Number Theory</i>. <a href="#bib">[G94]</a>. Guy defines the <b>height</b> of a happy number to be the number of iterations needed to reach $$1$$ and asks, can we give bounds on the least happy number of height $$h$$?

<table style="border:1px solid black;margin:0 0 2em 0;text-align:center;">
    <tr style="padding:0 0 .2em 0;">
        <td style="padding:0;"><b>Height</b></td>
        <td style="padding:0;"><b>Happy number</b></td>
        <td style="padding:0;"><b>Bit length</b></td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">0</td>
        <td style="padding:0;">1</td>
        <td style="padding:0;">1</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">1</td>
        <td style="padding:0;">10</td>
        <td style="padding:0;">4</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">2</td>
        <td style="padding:0;">13</td>
        <td style="padding:0;">4</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">3</td>
        <td style="padding:0;">23</td>
        <td style="padding:0;">5</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">4</td>
        <td style="padding:0;">19</td>
        <td style="padding:0;">5</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">5</td>
        <td style="padding:0;">7</td>
        <td style="padding:0;">3</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0">6</td>
        <td style="padding:0;">356</td>
        <td style="padding:0;">9</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">7</td>
        <td style="padding:0;">78999</td>
        <td style="padding:0;">17</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">8</td>
        <td style="padding:0;">3789 &times; 10<sup>973</sup> &minus; 1</td>
        <td style="padding:0;">3245</td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">9</td>
        <td style="padding:0;">78889 &times; 10<sup>(3789 &times; 10<sup>973</sup> &minus; 306)/81</sup> &minus; 1</td>
        <td style="padding:0;">>10<sup>973</sup></td>
    </tr>
    <tr style="padding:0 0 0.2em 0;">
        <td style="padding:0;">10</td>
        <td style="padding:0;">259 &times; 10<sup>[78889 &times; 10<sup>(3789 &times; 10<sup>973</sup> &minus; 306)/81</sup> &minus; 13]/81</sup> &minus; 1</td>
        <td style="padding:0;"><i class="far fa-surprise" style="font-size:1.4em;"/></td>
    </tr>
</table>

Inspired by this question, a series of works <a href="#bib">[GT03, CZ08]</a> explore happy numbers and provide methods for finding the happy numbers of any given height, in theory. The least happy numbers of heights 8, 9, and 10 were calculated by Grundman and Teeple in 2003 <a href="#bib">[GT03]</a>. A few years later, Cai and Zhou calculated the least happy numbers of heights 11 and 12 <a href="#bib">[CZ08]</a>. These values, along with their bit lengths, are provided in the table above. As you can see, they are beyond huge!

Cai and Zhou also introduced a theorem providing bounds on the number of digits in the least happy number of height $$k$$.


>For any $$k \ge 6$$, the total number of digits in $$\sigma_k,$$ the least happy number of height $$k,$$ is no more than $$[\sigma_{k-1}/81] + 4$$ and no less than $$[\sigma_{k-1}/81]$$.


Not only do we know that numbers with heights as low as 8 and 9 are massive &mdash; we also know that numbers with larger heights are even more massive. 

<h2>Seven Steps to Happiness</h2>

The least happy number of height 8 is 3,245 bits. The least happy number with height 9 is prohibitively large, as seen with even the laziest possible lower bound on an estimate of bit length that I put in the table. 

So, assuming that we will not encounter any numbers with bit lengths above 3,245 bits, we can replace the <code>while</code> loop in the code with a 7-iteration <code>for</code> loop, limiting the steps required in the worst-case. 

This may not be a particularly useful optimization, but I do think it is a mathematically interesting one. I included my implementation below. 

<script src="https://gist.github.com/alexanderwood/0ed7748b900c017caf4cb867b576a5fd.js"></script>

Long story short - <b>Happiness (or unhappiness) can be reasonably determined in seven steps!</b> 


<h2>Some Extra Happy Facts</h2>

Arthur Porges showed in 1945 that all unhappy numbers eventually land in the same cycle of eight numbers: $$4$$, $$16$$, $$37$$, $$58$$, $$89$$, $$145$$, $$42$$, $$20$$ <a href="#bib">[P45]</a>. This was the earliest reference to happy numbers I was able to locate. (Let me know if you find anything earlier).

<span class="image fit"><img src="{% link assets/images/20200506-cycle.svg %}" alt="The cycle of eight numbers that all unhappy numbers reach." /></span>

Variations on the Happy Numbers problem appeared again in the 60s in some books of mathematical puzzles <a href='#bib'>[D67, M66]</a>.

The Happy Numbers problem resurfaced when mathematician Reg Allenby's daughter encountered it at school. Allenby shared the problem with Guy, who included it as entry E34 in his book of open problems in number theory where he posed a series of interesting questions about happy numbers <a href="#bib">[G94]</a>. For instance, is possible to have arbitrarily long sequences of consecutive happy numbers (e.g. $$7839$$, $$7840$$, $$7841$$, $$7842$$)? El-Sedy and Siksek showed in 2000 that the answer is yes <a href="#bib">[ES00]</a>.

Another question asked by Guy is to determine how large the gap can be in the sequence of happy numbers.
Other research examines generalizations of happy numbers with different bases and powers <a href="#bib">[GT01]</a>.
An <a href="https://oeis.org/A007770" target="blank">additional unproven conjecture</a> postulates that there are infinitely many happy primes.


<h3>References</h3>
<section id="bib">
<ul style="font-size:0.8em;">
    <li style="color:#aaaaaa;">[CZ08] T. Cai and X. Zhou. On the Heights of Happy Numbers. Rocky Mountain J. Math. 38 (2008), no. 6, 1921&mdash;1926.</li>
    <li style="color:#aaaaaa">[D67] H. E. Dudeney. Problem 143 in <font style="font-style:italic;">536 Puzzles & Curious Problems</font>. New York: Scribner, (1967), pp. 43 and 258-259.</li>
    <li style="color:#aaaaaa">[ES00] E. El-Sedy and S. Siksek. On happy numbers. Rocky Mountain J. Mathematics 30 (2000), 565&mdash;570.</li>
    <li style="color:#aaaaaa">[G94] R. K. Guy. "Happy Numbers." §E34 in Unsolved Problems in Number Theory, 2nd ed. New York: Springer-Verlag, pp. 234&mdash;235, 1994. </li>
    <li style="color:#aaaaaa">[GT01] H. G. Grundman and E. A. Teeple. <i>Generalized happy numbers</i>. Fibonacci Quarterly 39 (2001), 462&mdash;466.</li>
    <li style="color:#aaaaaa;">[GT03] H.G. Grundman and E.A. Teeple. Heights of happy numbers and cubic happy numbers. Fibonacci Quart., 41 (2003), pp. 301&mdash;306.</li>
    <li style="color:#aaaaaa;">[GT07] H.G. Grundman and E.A. Teeple. Sequences of Consecutive Happy Numbers. Rocky Mountain J. Math. 37 (2007), no. 6, 1905&mdash;1916.</li>
    <li style="color:#aaaaaa;">[M66] Joseph S. Madachy. <i>Mathematics on Vacation</i>. Scribner, 1966 - Mathematical recreations</li>
    <li style="color:#aaaaaa;">[P45] Arthur Porges. A Set of Eight Numbers. <i>The American Mathematical Monthly</i>, Vol. 52, No. 7 (Aug. - Sep., 1945), pp. 379 &mdash; 382.</li>
</ul>
</section>