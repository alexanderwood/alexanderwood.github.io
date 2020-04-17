---
layout: post
title: Adding Google Analytics to Your Jekyll Website 
description: First post
excerpt_separator: <!--more-->
image: assets/images/20200417-banner.png 
image_caption: Ready &amp; waiting for my first visitors.
tags: jekyll google analytics tracking
---

Hey everyone! As a first post, I'm going through the steps I followed to get Google analytics up &amp; running on my website. 

<!--more-->

<h2>Getting started</h2>

First, if you haven't already, head over to <a href="https://analytics.google.com/" target="_blank">Google Analytics</a> and set up an account. 

Next, press the <i class="fas fa-cog"/> Admin cog in the bottom left and choose "Create Account."

<h2>Add Website Tracking to your webpage</h2>

After creating my account, I needed to link my Google Analytics Tracking ID to my web site. The tracking info is located in <code><i class="fas fa-cog"/> Admin > <i class="fas fa-angle-left"/><i class="fas fa-angle-right"/> Tracking Info > Tracking Code</code>. You should see a screen similar to the one below.

<span class="image fit"><img src="/assets/images/20200417-tracking-id.png" alt="Screenshot of Filter menu"></span>

Here I can find my unique Tracking ID and a code snippet provided by Google under the Global Site Tag heading. This tag needs to be in the <code><head></code> of every page I wish to track. Thankfully, Jekyll enables me to do this quite easily!

First, I created a new file called <code>analytics.html</code> and placed this in my <code>_include</code> directory. Next, I added the line <code>tracking_id: UA-00000000-0</code> to my <code>_config.yml</code> file. 

The Global Site Tag tracking code should appear as the first item in the <code><head></code> tag. My <code>_includes/head.html</code> looks as follows:

<pre><code>&lt;head>
    &#123;% <b>if</b> site.tracking_id %}&#123;% include analytics.html %}&#123;% <b>endif</b> %}
    
    &lt;!-- The rest of your head content here -->
&lt;/head>
</code>
</pre>

Since values in Liquid are "truthy," this prints <code>analytics.html</code> at the top of the <code><head></code> tag if <code>tracking_id</code> is anything except <code>nil</code> or <code>false</code>.


<h2>Create a Filter</h2>

Next, I wanted to make sure that no one could use my Analytics ID to send data from other sites. To do this, I created a custom filter to only include traffic that comes from my site in my Analytics.

Go to <code><i class="fas fa-cog"></i> Admin > <i class="fas fa-filter"></i> All Filters</code> and select <code>Add Filter</code>. Then,
<ul>
<li>Name your filter whatever you like.</li>
<li>Select Custom for filter type.</li>
<li>Next, choose the <code>Include</code> option.</li>
<li>From the Filter Field dropdown menu, pick <code>Hostname</code>.</li>
<li>Put your web address in the Filter Pattern field. Replace <code>.</code> with <code>\.</code> in order to interpret the period character literally. For instance, I typed <code>alexanderwood\.github\.io</code>.</li>
<li>Add <code>All Web Site Data</code> to Selected Views.</li>
<li>Press save!</li>
</ul>

This process will look something like the screenshot below.

<span class="image fit"><img src="/assets/images/20200417-filter.png" alt="Screenshot of Filter menu"></span>

I hope this was helpful!