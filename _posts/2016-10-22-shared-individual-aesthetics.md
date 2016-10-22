---
layout: post
title: Shared vs. Individual Aesthetics and Over-Engineering
author: Brandon
tags:
comments: true
---

I recently finished [The Goal](http://a.co/4fV4NYK) and I am also in the middle of [The Lean Startup](http://a.co/6wNibEh) and [Out of the Crisis](http://a.co/2Uix9du).  This has me thinking about several issues with regards the relationship between workers and the companies who employ them.  [A recent article](https://hackernoon.com/how-to-accept-over-engineering-for-what-it-really-is-6fca9a919263) ends with the lukewarm assertion that "Unfortunately, there's no universal answer for what always constitutes Over-Engineering and what doesn't."

I disagree with this.

There is no first order definition for over engineering, but there is a second order definition that I think fits the bill perfectly.  First I will need to exposite a little context.

In *The Goal*, the end strategy ends up subverting the need for local processes to have efficiency in isolation for the *goal* of having the system at large have the highest efficiency (throughput).  *The Goal* specifically treats the context of manufacturing, and at the end, the fictional plant manager receives a promotion to division manager.  Unfortunately, the story mostly stops here.  We don't get to see the strategy of optimizing the performance of the division.

I spent some time thinking about the adaptation of this to software development, which seemed to be much more artisanal in nature, and thus seemed to thwart the classifications laid out in *The Goal*.  However, after examination I think I know what's happening.

It is very common for individual developers to argue over the best way to approach a given problem.  I think there are two reasons for this.  I think each individual developer wants to be as productive as possible, and, altruistically, each individual developer has reasons for believing that these approaches would improve the productivity of other developers.  Or in a more Machiavellian sense, they could just not want to have to deal with any other approaches.  Either way, there is a distinct desire to optimize the productivity of the person in isolation.  Specifically, this optimization tends to be based in aesthetics.  We have the language of aesthetics in several areas of software development.  We have *beautiful webpages*, *software craftsmanship* and other similar language floating around the industry.  There is nothing inherently wrong with these concepts, but I think they speak to the desire of the individual to maximize their sense of productivity through the ability to freely express what is necessary to solve a problem.  This is the first step toward understanding what constitutes over engineering.

*The Goal* systematically demonstrates that performance of individual constituent parts of a system will not perform with 100% efficiency when the goal is to maximize the throughput of the system as a whole.  Looked at another way, the aesthetics of the business, or the values of the business, which is generally to grow, maximize throughput, and thus sales, do not correspond to the aesthetics of the individuals who comprise the system.

The definition of overengineering is extremely precise in light of these two things.  A component, or section of a system, or the system as a whole is overengineered when the work that contributed to creating that part or parts exceeded the contribution necessary increase to the throughput of the system as a whole.

One example could be the argumentation over language selection or technology stack which delays the delivery of an MVP.  Another example could be excessive rearrangement of the architecture of a subsystem.  As developers, we don't want to introduce bugs, or have bad UX, or poor performance, or any number of *personal concerns* be attached to our work.  Producing "poor work" in the name of business needs drives most developers insane, myself included.

This would seem to, at least in my experience, classify all attempts at software craftsmanship, and desire to produce something of quality as being on the precipice of over engineering.  More than this, it would color nearly every engineer I know as guilty of over engineering everything.  In reflection, I think this is actually accurate, and I will offer an analysis of the social conditions that create this pervasive atmosphere.

In nearly every environment I have worked in, including startup environments, as lead engineer or journeyman engineer, there was absolutely **no connection** between the work I did and the people who used it.  In fact, there was little communication of the value add to customers, or the ability to reflect on the delivery of this value add.  There is little socialization of business values with engineering, and when there is it is canned, artificial, and there is no quorum about business value and corporate value that intrinsically includes engineering.

This leads to a categorically Marxist dissonance concerning working to produce something you will not see the value of in either your own usage, or the vicarious usage of others.  There is literally no ownership over the value produced.  Instead it is traded for capital.  However, developers are not production line workers.  We work in a medium of expression, and a little bit of our soul goes out with each line of code we write.  Giving this away is hard to justify at any price.  So we built adjunct guild-like structures where we confer on best practices, and we develop determinations of personal efficiency, whether that's ability to code for 12 hours straight, the ability to write code any developer can read, the ability to write the shortest possible code, etc.  These aeshetics arise as a way of resolving the internal dissonance for trading soul for capital.  But there is no reason that shared aesthetics, business aesthetics, cannot be established.

This is how you stop over engineering.  You socialize and build consensus between business aesthetics and values with the people who are responsible for delivering on those aesthetics.  Because, at least for me, if I can see the joy of someone using my horrendously buggy software, I will be more concerned with continuing to deliver them value than I will be with its performance, its relative correctness, its code beauty, or any other artificial metrics that I use to measure the cost of my soul that I spent to create it.

Over engineering is definable in relativistic terms.  The culture of over engineering condenses from the a culture of toxic capitalistic separation of values.  The solution isn't necessarily a Marxist revolution, but a simple humanistic evolution that realizes that producers of software are human, and have human contributions.  When they see the value of their contributions, they will stop inventing artificial metrics that cause local optima and detract from the throughput of the system as a whole.

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    var PAGE_URL = "http://www.brandonkeown.com/2016/10/lenses-for-fun-and-profit.html";
    var PAGE_IDENTIFIER = "lenses-for-fun-and-profit";

    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };

    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');

        s.src = '//theqabalist.disqus.com/embed.js';

        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
{% endif %}
