---
layout: post
title:  "How can i accept and run user's code securely on my web app?"
date:   2021-07-15 20:02:00 +0700
categories: [python, django, security]
---
**Contents**
* TOC
{:toc}



<!DOCTYPE html>
<html lang="en" itemscope itemtype="http://schema.org/Blog">
	<head>
    <title>Linear Regression In Pictures - adit.io</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta name="viewport" content="width=320" />
    <meta itemprop="name" content="adit.io">
    <meta itemprop="description" content="Aditya Bhargava's personal blog.">
    <meta name="description" content="Aditya Bhargava's personal blog.">
    <link href="https://fonts.googleapis.com/css?family=Ovo|Lato:300,900" rel="stylesheet" />
    <link rel="stylesheet" href="/css/reset.css">
    <link rel="stylesheet" href="/css/disqus.css">
    <link rel="stylesheet" href="/css/style.css">
    <link rel="stylesheet" href="/css/highlighting.css">
    <link rel="stylesheet" href="/css/bootstrap-grid.min.css">
    <link rel="stylesheet" media="screen and (max-device-width: 800px)" href="/css/mobile.css" />

    <script type="text/javascript" src="/js/jquery.js"></script>

	</head>
        <body>
    <ul id="menu">
      <li><a href="/index.html">&gt; adit.io</a></li>
      <li class="highlight"><a href="http://amzn.to/29rVyHf">Buy my book! Grokking Algorithms</a></li>
    </ul>
    <div id="container" class="container">
      <div class="row">
        <div id="content" class="span12">
          <ul id="sections">
          <h2>Contents</h2>
            <li><a href="#what-is-linear-regression?">What Is Linear Regression?</a></li>
            <li><a href="#the-cost-function">The Cost Function</a></li>
            <li><a href="#gradient-descent">Gradient Descent</a></li>
            <li><a href="#putting-it-all-together">Putting It All Together</a></li>
            <li><a href="#recap">Recap</a></li>
        </ul>
          <h1>Linear Regression In Pictures</h1>
<time>Written February 20, 2016</time>

<p>I have been learning machine learning with Andrew Ng&#39;s excellent <a href="https://www.coursera.org/learn/machine-learning/">machine learning course on Coursera</a>. This post covers Week 1 of the course.</p>

<p>You should read this post if week 1 went too fast for you. I cover the same stuff, but slowed down and with more images!</p>

<p>I&#39;ll talk about:</p>

<ul>
<li>what linear regression means</li>
<li>cost functions</li>
<li>gradient descent</li>
</ul>

<p>These topics are the foundation for the rest of the course.</p>
<h2 id=what-is-linear-regression?>What is linear regression?</h2>
<p>Suppose you are thinking of selling your home. Different sized homes around you have sold for different amounts:</p>

<p><img src="/imgs/regression/homes.png" alt=""></p>

<p>Your home is 3000 square feet. How much should you sell it for? You have to look at the existing data and predict a price for your home. This is called <em>linear regression</em>.</p>

<p>Here&#39;s an easy way to do it. Look at the data you have so far:</p>

<p><img src="/imgs/regression/home_prices_plotted.png" alt=""></p>

<p>Each point represents one home. Now you can eyeball it and roughly draw a line that gets pretty close to all of these points:</p>

<p><img src="/imgs/regression/eyeballed_line_good.png" alt=""></p>

<p>Then look at the price shown by the line, where the square footage is 3000:</p>

<p><img src="/imgs/regression/eyeballed_line_3000_prediction.png" alt=""></p>

<p>Boom! Your home should sell for $260,000.</p>

<p>That&#39;s all there is to it. You plot your data, eyeball a line, and use the line to make predictions. You need to make sure your line fits the data well:</p>

<p><img src="/imgs/regression/good_vs_bad_fit.png" alt=""></p>

<p>But of course we don&#39;t want to eyeball a line, we want to compute the exact line that best &quot;fits&quot; our data. That&#39;s where Andrew Ng and machine learning comes in.</p>

<p>How do you decide what line is good? Here&#39;s a bad line:</p>

<p><img src="/imgs/regression/eyeballed_line_bad.png" alt=""></p>

<p>This line is way off. For example, according to this line, a 1000 sq foot house should sell for $310,000, whereas we know it actually sold for $200,000:</p>

<p><img src="/imgs/regression/predicted_vs_actual.png" alt=""></p>

<p>Maybe a drunk person drew this line...it is $110,000 dollars off the mark for that house. It is also far off all the other values:</p>

<p><img src="/imgs/regression/predicted_vs_actual_bad_line.png" alt=""></p>

<p>On average, this line is $73,333 off ($110,000 + $70,000 + $40,000 / 3).</p>

<p>Here&#39;s a better line:</p>

<p><img src="/imgs/regression/predicted_vs_actual_good_line.png" alt=""></p>

<p>This line is an average of $8,333 dollars off...much better. This $8,333 is called the <em>cost</em> of using this line. The &quot;cost&quot; is how far off the line is from the real data. The best line is the one that is the least off from the real data. To find out what line is the best line, we need to use a <em>cost function</em>.</p>
<h2 id=the-cost-function>The cost function</h2>
<p><img src="/imgs/regression/cost_function.png" alt=""></p>

<p>First some foundational math. We are drawing a line. Here&#39;s what the equation of a line looks like:</p>

<p><img src="/imgs/regression/equation_and_line.png" alt=""></p>

<p>The first number tells you how high the line should be at the start:</p>

<p><img src="/imgs/regression/first_number.png" alt=""></p>

<p>The second number tells you the angle of the line:</p>

<p><img src="/imgs/regression/second_number.png" alt=""></p>

<p>So we need to come up with two pretty good numbers that make the line fit the data. Here&#39;s how we do it:</p>

<ol>
<li>Pick two starting numbers. These are traditionally called <code>theta_0</code> and <code>theta_1</code>. Zero and zero are a good first guess for these.</li>
<li>Draw a line using those numbers:</li>
</ol>

<p><img src="/imgs/regression/first_guess_for_theta.png" alt=""></p>

<p>(Yes, it is a line where all the predicted values are zero. It is still a fine first guess, we are going to keep improving it).</p>

<ol>
<li>Calculate how far off we are, on average, from the actual data:</li>
</ol>

<p><img src="/imgs/regression/distances_from_first_guess.png" alt=""></p>

<p>This is called the <em>cost function</em>. You pass <code>theta_0</code> and <code>theta_1</code> into the function, and it tells you how far off that line is (i.e. the <em>cost</em> of using that line).</p>

<p>I&#39;ll use Octave code, since that&#39;s what the course uses.</p>
<pre class="highlight haskell"><code><span class="o">%</span> <span class="n">x</span> <span class="n">is</span> <span class="n">a</span> <span class="n">list</span> <span class="kr">of</span> <span class="n">square</span> <span class="n">feet</span><span class="o">:</span> <span class="p">[</span><span class="mi">1000</span><span class="p">,</span> <span class="mi">2000</span><span class="p">,</span> <span class="mi">4000</span><span class="p">]</span>
<span class="o">%</span> <span class="n">y</span> <span class="n">is</span> <span class="n">the</span> <span class="n">corresponding</span> <span class="n">prices</span> <span class="n">for</span> <span class="n">the</span> <span class="n">homes</span><span class="o">:</span> <span class="p">[</span><span class="mi">200000</span><span class="p">,</span> <span class="mi">250000</span><span class="p">,</span> <span class="mi">300000</span><span class="p">]</span>
<span class="n">function</span> <span class="n">distance</span> <span class="o">=</span> <span class="n">cost</span><span class="p">(</span><span class="n">theta_0</span><span class="p">,</span> <span class="n">theta_1</span><span class="p">,</span> <span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">)</span>
  <span class="n">distance</span> <span class="o">=</span> <span class="mi">0</span>
  <span class="n">for</span> <span class="n">i</span> <span class="o">=</span> <span class="mi">1</span><span class="o">:</span><span class="n">length</span><span class="p">(</span><span class="n">x</span><span class="p">)</span> <span class="o">%</span> <span class="n">arrays</span> <span class="kr">in</span> <span class="n">octave</span> <span class="n">are</span> <span class="n">indexed</span> <span class="n">starting</span> <span class="n">at</span> <span class="mi">1</span>
    <span class="n">square_feet</span> <span class="o">=</span> <span class="n">x</span><span class="p">(</span><span class="n">i</span><span class="p">)</span>
    <span class="n">predicted_value</span> <span class="o">=</span> <span class="n">theta_0</span> <span class="o">+</span> <span class="n">theta_1</span> <span class="o">*</span> <span class="n">square_feet</span>
    <span class="n">actual_value</span> <span class="o">=</span> <span class="n">y</span><span class="p">(</span><span class="n">i</span><span class="p">)</span>
    <span class="o">%</span> <span class="n">how</span> <span class="n">far</span> <span class="n">off</span> <span class="n">was</span> <span class="n">the</span> <span class="n">predicted</span> <span class="n">value</span> <span class="p">(</span><span class="n">make</span> <span class="n">sure</span> <span class="n">you</span> <span class="n">get</span> <span class="n">the</span> <span class="n">absolute</span> <span class="n">value</span><span class="p">)</span><span class="o">?</span>
    <span class="n">distance</span> <span class="o">=</span> <span class="n">distance</span> <span class="o">+</span> <span class="n">abs</span><span class="p">(</span><span class="n">actual_value</span> <span class="o">-</span> <span class="n">predicted_value</span><span class="p">)</span>
  <span class="n">end</span>
  <span class="o">%</span> <span class="n">get</span> <span class="n">how</span> <span class="n">far</span> <span class="n">off</span> <span class="n">we</span> <span class="n">were</span> <span class="n">on</span> <span class="n">average</span>
  <span class="n">distance</span> <span class="o">=</span> <span class="n">distance</span> <span class="o">/</span> <span class="n">length</span><span class="p">(</span><span class="n">x</span><span class="p">)</span>
<span class="n">end</span>
</code></pre>
<p>I&#39;ll also show the math formula they use for this. It is pretty much the same thing:</p>

<p><img src="/imgs/regression/cost_function_math_formula.png" alt=""></p>

<p>In this formula, <code>h(x)</code> represents the predicted value:</p>

<p><img src="/imgs/regression/h_x_formula.png" alt=""></p>

<p>It&#39;s just the formula for our line! If we plug in a specific value for <code>x</code>, we will get a value for <code>y</code>.</p>

<p>So <code>h(x) - y</code> gives us the difference between the predicted value and the actual value.</p>

<p>For a given value of <code>theta_0</code> and <code>theta_1</code>, the cost function will tell you how good those values are (i.e. it will tell you how far off your predictions were from the actual data). But what do we do based on that information? How do we find the values of <code>theta_0</code> and <code>theta_1</code> that will draw the best line? By using <em>gradient descent</em>.</p>
<h2 id=gradient-descent>Gradient descent</h2>
<p><img src="/imgs/regression/gradient_descent.png" alt=""></p>

<p>Let me start with a simpler version of gradient descent, and then move on to the real version. Suppose we decide to leave <code>theta_0</code> at zero. So we experiment with what value <code>theta_1</code> should be, but <code>theta_0</code> is always zero. Now you can try various values for <code>theta_1</code>, and you will end up with different costs. You can plot all of these costs on a graph:</p>

<p><img src="/imgs/regression/cost_for_theta_1.png" alt=""></p>

<p>So for example, using a <code>theta_1</code> value of 75 is better than a value of 160. The cost is lower:</p>

<p><img src="/imgs/regression/cost_for_theta_1_callouts.png" alt=""></p>

<p>Here are the corresponding lines (remember, <code>theta_0</code> is zero in these lines):</p>

<p><img src="/imgs/regression/75_vs_160.png" alt=""></p>

<p>We can see that the line on the left seems to fit the data better than the line on the right, so it makes sense that the cost of that line is lower. And from this graph it looks like <code>theta_1 = 75</code> gives us the lowest cost overall:</p>

<p><img src="/imgs/regression/lowest_cost.png" alt=""></p>

<p>Since it is the lowest point in this graph. So with all the costs graphed out like this, we just need to find the lowest point on the graph, and that will give us the optimal value of <code>theta_1</code>!</p>

<p>Gradient descent helps us find the lowest point on this graph. You start with a value for <code>theta_1</code>, and update it iteratively till you arrive at the best value. So you can start at <code>theta_1 = 0</code>. Then you have to ask, should I go left or right?</p>

<p><img src="/imgs/regression/guess_theta_1_zero.png" alt=""></p>

<p>Well, we want to go <em>down</em>, so lets go right a small step:</p>

<p><img src="/imgs/regression/go_right.png" alt=""></p>

<p>This is the new value for <code>theta_1</code>. Again we ask, should we go left or right? At each step, you need to head downward, till you get to a point where you&#39;re as low as you can go:</p>

<p><img src="/imgs/regression/arrows_to_bottom.png" alt=""></p>

<p>This is <em>gradient descent</em>: going down bit by bit till you hit the bottom. How do you figure out which way is down? The answer will be obvious to calculus experts but not so obvious for the rest of us: you take the derivative at that point. This is the part that I&#39;ll gloss over and just give you a formula to apply. But the important bit to know is, if you take the current value of <code>theta_1</code> and add the derivative at that point, you will go <em>down</em>. You just do that a bunch of times (say 1000 times) and you will hit bottom!</p>

<p>That was the simpler view, now lets get back to the original problem. In this problem, we need to know the optimal values for both <code>theta_0</code> and <code>theta_1</code>. So the graph looks more like this:</p>

<p><img src="/imgs/regression/3d_plot_thetas.png" alt=""></p>

<p>So you can see having <code>theta_0 = 200000</code> and <code>theta_1 = 200</code> would be really bad, but <code>theta_0 = 100000</code> and <code>theta_1 = 50</code> would be okay.</p>

<p>It is still a bowl with a low point, it is just in 3d because now we are considering <code>theta_0</code> as well. But the idea stays the same: start somewhere in the bowl and just keep taking steps till you are at the bottom!</p>

<p>This is how you find the line with the lowest cost with gradient descent. You find the <code>theta_0</code> and <code>theta_1</code> values that give you the best fitting line by starting with a guess and incrementally updating that guess to make the cost lower and lower.</p>

<p>Here&#39;s the formula for gradient descent:</p>

<p><img src="/imgs/regression/gradient_descent_math_formula.png" alt=""></p>

<p>They are partial derivatives used in two update rules: one for <code>theta_0</code> and another for <code>theta_1</code>. The two are almost the same, except the <code>theta_1</code> derivative has that extra <code>x</code> at the end.</p>
<h2 id=putting-it-all-together>Putting it all together</h2>
<p><a href="https://gist.github.com/egonSchiele/2405a8c020e3d2fbdd5a">The Octave code can be found here</a>. <a href="https://gist.github.com/derekmcloughlin/87c33ec383d951060573">Here&#39;s an R version</a> thanks to <a href="https://github.com/derekmcloughlin">Derek McLoughlin</a>. Note: I used something called <em>feature scaling</em> here, so the square feet values will be <code>[1, 2, 4]</code> and not <code>[1000, 2000, 4000]</code> as expected. This just makes gradient descent work better here.</p>

<p>We started with these data points:</p>

<p><img src="/imgs/regression/home_prices_plotted.png" alt=""></p>

<p>Using gradient descent, we find that <code>theta_0 = 175000</code> and <code>theta_1 = 32.143</code> give us the best fitting line, which costs $7,142.9 and looks like this:</p>

<p><img src="/imgs/regression/best_line.png" alt=""></p>

<p>(our eyeballed line was really close, it had a cost of $8,333.33).</p>

<p>And know you know that you can sell your 3000 square foot home for $271,430 dollars!</p>

<p>One final note: Octave includes a function called <code>fminunc</code> that does the hard work of gradient descent for you...you just need to give it a cost function. <a href="https://gist.github.com/egonSchiele/07a41ebb7654e62f6498">Here&#39;s an example</a>.</p>

<p><a href="http://adit.io/posts/2016-03-13-Logistic-Regression.html">Read the followup to this post (logistic regression) here</a>.</p>
<h2 id=recap>Recap</h2>
<ul>
<li>Linear regression is used to predict a value (like the sale price of a house).</li>
<li>Given a set of data, first try to fit a line to it.</li>
<li>The cost function tells you how good your line is.</li>
<li>You can use gradient descent to find the best line.</li>
<li>Take the Coursera course <a href="https://www.coursera.org/learn/machine-learning/">here</a>! It is great.</li>
</ul>

<p><img src="/imgs/regression/home_for_sale.png" alt=""></p>


        </div>
      </div>
      <div class="row" style="padding-top: 50px;">
        <a href="/privacy.html">Privacy Policy</a>
      </div>
    </div>
		</body>
	</html>

