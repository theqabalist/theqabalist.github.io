---
layout: post
title: Critique of "Out of the Tarpit"
author: Brandon
tags:
comments: true
---

I just finished reading the paper entitled [Out of the Tarpit](http://shaffner.us/cs/papers/tarpit.pdf) and I wanted to share some critiques I have of it.

My critiques are mainly philosophical and linguistic in nature.  While I agree that complexity is a large contributing factor to the software problems today, I take issue with the way in which they specifically went about illustrating it.  They separated things into essential state (and complexity) and accidental state (and complexity).  They then bring up what I think is an incredibly dubious idea of “required accidental complexity.”  If they were going to go this route they should have chosen a better word, such as ancillary complexity, rather than picking two words that have opposing connotations.

This is not even the main critique.  The real problem is one that I don’t think the authors are even aware of, and is postmodern in nature.  Take for instance something like communication.  I think it’s arguable that communication support mechanisms are as important to semantics as the “platonic” essence of the communication itself.  For example, if I were to communicate to you over a phone that dropped every other word, that would not convey the “essential” meaning or semantics.  Therefore the support system is REQUIRED for semantic reception.  Similarly, if I spoke 1/10 of average speed, you would fail to receive the “essence” of the communication.  Carrier and payload, surface and deep structure are **both** required for semantics.  You simply cannot escape this.  As a preview to later in my critique, even in the most pristine DSL, you still have to type keys into a keyboard onto a virtual document representing some sort of executable structure.  This is a carrier.  The question of how much carrier is required to convey a given deep structure is something that I think is not naively derivable from just the deep structure.  I think they influence each other, this is the heart of the post modern critique of language.

To make the point concrete, I can apply the abstract critique to their own approach.  They argue that there should be a separation of concerns with regards to essential logic and state, and accidental logic and state.  This is, I think, a fancy way of saying, have a safe language, and an unsafe runtime, or have a language and an interpreter.  However, their own definition of accidental complexity is “if a user doesn’t know what it is, such as a thread pool or loop counter” then it’s accidental complexity.  Then they go on to define a language for the communication of “essential state” that involves joins, unions, projections, etc from the relational algebra.  Try as you might, I am pretty sure structuring programs as relational algebra necessarily means that my mother wouldn’t know what “project_away a field from a relvar” means.  This is therefore accidental complexity (by their own definition) in a solution that was supposed to eschew it.

This reinforces my point that carrier and payload of a message are intertwined.  You can focus on reducing carrier weight, to the lightest possible point, which would be the lisp/scheme style DSL + interpreter, and I think this is a smart approach.  However, I think the differentiation of platonic intent and surface structure is dangerous to formalize, and I think their own approach bit them as a performative contradiction.  They essentially argued restricting yourself from a general purpose language to a relational language plus a relational runtime.  But that decision was arbitrary, and was most likely done because the author favors the relational model aesthetically.  The relational model introduces extra structural concepts on top of the business domain.  The only way to strip away as much “accidental” complexity as theoretically possible is to ensure that there is no syntax that doesn’t relate directly to the underlying structure being conveyed.  In other words, if there is syntax, it is necessary it be related to the domain that is being conveyed.  Union, join, project are the syntax of the relational algebra (or possibly a populist politician), and therefore convolute the message being sent in the same way a TCP handshake convolutes the message being sent (though TCP doesn’t interweave with its payload at all.)

I part with the authors in thinking that we should all program in a relational model for large scale applications, and given that I think that was the heart of their argument (even though they had a lukewarm closing that self-deprecated their argument), I can’t say I agree with the majority of the paper.  Moreover, with regards to their discussion of complexity, I think they used a naive method of discussing complexity that itself complected (to borrow the term from Rich Hickey) the understanding of how to deal with complexity by not addressing it holistically (in addition to atomistically).

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    var PAGE_URL = "http://www.brandonkeown.com/2016/11/tarpit-critique.html";
    var PAGE_IDENTIFIER = "tarpit-critique";

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
