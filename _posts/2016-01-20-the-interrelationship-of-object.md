---
layout: post
title: The Interrelationship of Object Orientation and Functional Programming
date: '2016-01-20T14:13:00.000-06:00'
author: Brandon
tags:
modified_time: '2016-01-22T12:26:07.897-06:00'
blogger_id: tag:blogger.com,1999:blog-6424479588103664299.post-9037757009779215322
blogger_orig_url: http://www.brandonkeown.com/2016/01/the-interrelationship-of-object.html
---

I once was a zealot of object oriented programming.  Then I was born anew into the fascinating mathematical world of functional programming.  It has been my journey in the past six or so months to reconcile the two.  I had the pleasure of conversing over email with Dr. Alan Kay to get his opinion on the matter, because as far as I can tell, he is still one of the most vociferous proponents of message passing and late binding (and why wouldn't he be?)  The following treatise will be my attempt to contextualize the two modalities so that they can be thought of as a single paradigm, and not two paradigms.

First I would like to start with a disclaimer.  I have never been a really detailed oriented person.  While I have a gift of memorization of certain details, most of the times, I focus on the patterns that connect, and so you will not find rigorous mathematical proofs or anything like that from me.  You will instead find ideas that may not even have the formalizations yet to be borne out of proofs.  This means I will hand wave, and I will hand wave hard.  I realize that hand waving puts me at risk of missing important discriminatory details.  Such is life, and I will revise my conceptualizations if my hand waving is contradicted.

Object Orientation
==================
I think it's important to talk about the strengths of this approach as were originally envisioned in the 60s (rather than the panacea in the 80s/90s).

There are uncertainties in the world.  How do we address these uncertainties?  We just hope for the best.  Literally.  Using a message passing attitude, we send messages to receivers from whom we should really not expect anything in return because we don't know if they received the message or not.  This is put to the extreme in languages like Erlang where all messages are sent asynchronously.

Some languages, most languages frankly, which claim to be object oriented, lead us into the false trap of synchronicity, or the idea that we can count on the return value being a meaningful acknowledgment from the receiver of the message we send.

Before I finish that thought, I would like to posit that in strict OO languages, like Ruby for instance, there are 3 meaningful orthogonal categories of objects:

* *value objects*, which are immutable and increase the richness of the languages primitives by creating new primitives at a different level of abstraction
* *stateful objects*, such as random number generators
* *external interface service objects*, which help us communicate with the outside world.

It is possible that some objects become a conflation of the three, but I would argue that decomposition into one of these three types will help keep the system easy to think about (I'm looking at you, Active Record object).

The only object which you should send multiple subsequent messages are value objects.

<pre><code class="ruby">list.map(&:capitalize).first[0...-1]</code></pre>

in Ruby is a good example of something that is the transformation of value, and not the communication of said value.  Chaining these messages is tantamount to expressing a contained algebra, and not object communication per se.

The other two types of objects should be assumed to be stand alone processes which will communicate their states by themselves.  While random number generation is somewhat fast, imagine you have a prime number generator.  Each time you want to get the next prime, you will wait longer and longer.  Thus the prime number generator should communicate its findings to a listener asynchronously.

Most understandings of single responsibility principle are deconstructive.  Given a massive heaping mound of junk that is a giant method, we tease apart responsibilities until one remains.  However, the preceding paragraph lays out a constructive framework for methods, namely that every method should have one calculation, and one-to-n asynchronous communications of that calculation.  That is it.  The moment you wait for a message to return and you switch on it, you have introduced temporal coupling into your method, and certainly it is now doing more than one thing (sending a message, and then deciding which next message to send).

<pre><code class="ruby">def magic_string_manipulation(s):
  new_s = s.upcase.reverse
  @listener.manipulated(new_s)
end</code></pre>
Here is an example of a method with a single (compound) responsibility, it communicates the new value of s to a listener.  Here is how that method can go wrong:

<pre><code class="ruby">
def magic_string_manipulation_bad(s):
  new_s = s.upcase.reverse
  printed = print_something(new_s)
  if (printed)
    @listener.manipulated(new_s)
  end
end</code></pre>
This example sends a message to what is ostensibly a string printer, and waits for a response.  This subsequent code, the if statement, is now spatially coupled, and method arguably has three responsibilities which is to first print something, the structure of deciding of whether or not to send a message, and the actual code to send the message.  The similar method

<pre><code class="ruby">def magic_string_manipulation_alt(s):
  new_s = s.upcase.reverse
  print_something(new_s)
  @listener.manipulated(new_s)
end</code></pre>
Is an acceptable variant because the responsibility of the method is to communicate the new value to the <i>interested parties</i>, which happen to be two different objects.   This is generalized to sending the message to n parties over a loop of observers.  What makes this acceptable is that there is no temporal coupling.  I can call print_something and manipulated in whatever order I want.

One calculation, one communication (to possibly n listeners).  This makes a method with one responsibility.

As a final note, message passing does not require methods on objects per se.  There are other notable constructions of message passing (such as CSP), which allow for the organization of code around a different atom than the method.

Functional Programming
======================
Let's switch gears for a moment and talk about the strengths of functional programming.

Functional programming has its foundation in mathematics.  In most simple mathematics, there is no concept of time.  You have what is called equational reasoning.  An equation such as x = 5 is true now, and 5 minutes from now, etc.  Leveraging this knowledge allows us to do decompositions of equations such as y = 5x<sup>2</sup> + 6x + 7 where, because of the commutativity and associativity of the addition operator over the real numbers, this can be deferred to 3 different processes (one to calculate each term).  Many gains can be made when there simply is no concept of time.

Similar to the concept of equational reasoning is the concept of referential transparency.  I can say y = 5x<sup>2</sup> + z + 7 where z = 6x and it's exactly the same as the previous equation because, once again, there is no time.  Whether I use z symbolically or 6x is irrelevant, and I can optimize computational resources based on this substitutability.

Finally, also in reference to the both equational reasoning and referential transparency is the concept of denotational semantics.  Denotational semantics is essentially the idea that the semantics of the program can be inferred from the semantics of its constituent parts (denotations).  If y is our program, we can say that the meaning of y is 5x<sup>2</sup> + 6x + 7.  If we are also given x, then we can further denote the semantics of the program.  This is an incredibly helpful model of computation as the compositional capabilities of purely denotative code are endless.  If you can write every intent as a denotation, you can achieve infinite composition, which means infinite code reuse, which is the "gold standard" of some aspects of programming.

But you may notice that time and again, I have said that there is no time in these models, and that is true.  Once time is introduced, the concept of denotational semantics and referential transparency start to get muddy.  If x = 0 at time 0 and x = 5 at time 1, we cannot say what y equals universally.  This is one aspect of time.  The other aspect of time is communication.  A system who merely calculates something but does not communicate the calculation fails to achieve anything at all.  Communication is a hairy problem because we have to decide to what or whom we are communicating.  If we communicate to another process we own, we can ensure nice denotational semantics.  If we communicate with a process we do not own, we lose this.

Bureaucracy vs Democracy
========================
Some useful models for the "at scale" conceptualizations of functional programming already exist at the social level.  These can be helpful when understanding how OO and FP relate to each other, and when it could be appropriate to use either.

You may have noticed at this point that original object orientation, with its emphasis on message passing and late binding is very good at communication.  It is the ultimate democracy when done right.  You may have a few infrastructural units that ensure that messages are delivered, but other than that, the behavior of the system arises from the communication of the constituent parts.  This is like a slime mold, no centralized structure, like a brain, but high intelligence (adaptability).

Functional programming, with its rigid and formal decomposition is the perfect hierarchy.  The main program delegates to constituent calculations and behaves in a highly formal way.  This is more like the army, built to do one thing, and one thing well, and not very adaptive to change.  It is also like the governmental bureaucracy, a hierarchy of units that perform one task and delegate subtasks in the service of some set of goals (or one goal if you consider the "law" one goal).

Bureaucracies are good at representing closed systems, or systems in which all inputs are formalized (if not known in advance), and their transformations are formalized so the output is essentially a transformation or extrapolation of the input.  Democracies are good at representing open systems, where inputs can change, and cannot be formalized, and decisions have to be made about new or different requirements as the system evolves over time and agents are introduced or removed.

I would like to give a nod to the fact that it is theoretically possible for bureaucracies to adapt to change by imposing a supervisory bureaucracy to manage the structure of the managed bureaucracy.  This means that it is possible to manage "degrees of temporal freedom" in systems that otherwise represent closed systems.  Similar to the concept of position, velocity, and acceleration, you can have data, transformations, and meta-transformations.  Each level of abstraction on the transformation allows a degree of temporal freedom on the underlying transformation, which can allow a closed system to be more flexible.  Which is to then say that functional programming systems can have temporal formalizations allowing them to change with time (and thus current conditions), but ultimately all functional systems require formalization, and formalizing at a meta-temporal level of abstraction can be quite difficult for even the most gifted person.  One method of managing temporal freedom in this way in a functional way is through functional-reactive programming.  When you have streams of streams, you then have the meta-transformational layer as well.

The Obligatory Monad Detour
===========================
The manner by which the richest functional systems communicate change is typically through the usage of a mathematical construct known as a monad.  I'm going to hand wave the fuck out this next part so bear with me, this is mainly to communicate to people who have not studied category theory.

Imagine you have two worlds, a world of green objects, and a world of blue objects.  If there is a mapping between these two worlds such that for every green object there is a blue object, and for every green relationship there is a blue relationship, you have a functor.  Functors preserve objects and relationships from one world to another (world = category).  Now imagine you have a second functor that maps blue back to green, but differently from the first, so the second functor is not the inverse of the first functor.  Now you have a way to go from green to green (like a regular green relationship), but going through blue.  This traversal and transformation, by way of the blue world is a formalization of the act of communication.  In Haskell, it is through observation of this second "world" that actual communication with the real world happens.  However, observation is not always required, sometimes the second world merely hides the accumulation of effects (such as in random number generation).  But for either case, monads exist as a formalization of different types of effecting mechanisms.

Get ready for REAL handwaving here.  Monads are similar to vector mathematics and vector calculus, in which you have n orthogonal dimensions that you are manipulating at the same time.  A side effect can be modeled as a consequent orthogonal calculation (sometimes with no meaningful input or output) on a dimension orthogonal to the original one.  A simple monad is like a two dimensional vector, and a transformer allows for n-dimensional vectors, which all communicate their respective side effects to the correct dimension.

This understanding is still a work in progress for me, as it does not directly translate intuitively to comonadic structures, but I believe it is sufficient as to be useful to anyone who understands it.

The Reconciliation
==================
Alan Kay said somewhere that CLOS (Common Lisp Object System) was the closest thing he could think of at the time to his original conceptualization of message passing object orientation.  Lisp lends itself particularly well to functional programming techniques, if not being a pure language like Haskell.  In fact, SICP is taught in Scheme, a dialect of Lisp, and all of the major foundations of object orientation are introduced, if under other names.  Additionally, Lisp's strong history with homoiconic macros introduces a level of power and expressiveness that allows for symbolic manipulations, which help with adaptation to change over time with uncertainty.  This is why Lisp is employed so often in AI and machine learning, because it can do symbolic manipulations with expressive ease.

This is not to say run out and use Lisp.  The point is that Lisp is a multiparadigm language, and in some of its best forms embodies the strengths of both an object/message passing model, and a functional calculation model.  Most object oriented languages excel at inter-object communication; dynamic OO languages excel at late binding.  But communication is one half of the coin.  Good strong calculation semantics are given from the world of functional programming, employing them to ensure that the <i>values</i> that are passed in messages are correct is something that suits it very well.  Calculate using FP, communicate using OO.  Or as a wise friend of mine once said to me, but it didn't click at the time "OO shell, FP internals".

If you are using a language like Haskell, you are at the somewhat forefront of mathematical formulations of communication.  Math always lags phenomena because it attempts to formally describe it.  OO systems can produce phenomena that math will not be able to describe formally for a very long time (neural networks are a good example).  This limitation is not very limiting however, in that it is possible to build a good deal of systems that are required today without emergent behaviors.  Additionally, there are some systems that are completely closed with well known constraints.  You don't want your brake system in your car or any sort of life support system to have "emergent" behavior.  You want to know, formally, that it works, and FP heavy languages can give you that.

Now it's time for the old, boring, played out platitude: in computation, when things are turing equivalent, there is no "right" way, only tradeoffs.  Assess your tradeoffs and execute accordingly.
