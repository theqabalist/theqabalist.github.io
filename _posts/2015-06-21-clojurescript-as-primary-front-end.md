---
layout: post
title: ClojureScript As a Primary Front End Development Tool
date: '2015-06-21T15:01:00.002-05:00'
author: Brandon
tags:
modified_time: '2015-06-21T15:02:08.259-05:00'
blogger_id: tag:blogger.com,1999:blog-6424479588103664299.post-3069671270608446506
blogger_orig_url: http://www.brandonkeown.com/2015/06/clojurescript-as-primary-front-end.html
---

<span style="color: #999999;">Note from the author: This treatise has been modified slightly from a proposal written to a former company that I worked for to generalize it.  Areas of specific technology considerations pertinent to the company this was written for will either be removed, or annotated to indicate such.</span></i>

Abstract
========
ClojureScript is a relatively young (2011), but very promising port of Clojure (2007) that targets the Javascript browser and <i>Node.js</i>runtimes.  It provides nearly all of the core semantics of Clojure, and brings strong functional programming fundamentals and features to the development of Javascript powered systems.  Usage of the currently existing infrastructure, tool chain, and interoperability features of ClojureScript allow for the production of a superiorly maintainable code base while maximizing performance by separating the concerns of expression of programmatic semantics from the implementation concerns of the underlying runtime.  This means less code, properly framed programmatic semantics, and superior execution over a typical ES5 development effort (that expresses the same semantics).  While not devoid of risks, this proposal aims to elucidate why ClojureScript and its accompanying toolset should be the tool of choice for development of new front-end systems.

Linguistic and Philosophical Differences
========================================
Javascript and Clojure come from very different linguistic backgrounds.  While Javascript has a few functional semantics, Javascript is primarily an imperative language with constructs that allow functional programming if desired.  Clojure is a functional language that has a few imperative constructs to interoperate with target languages.

Javascript is like any other imperative language.  It has a few gotchas that separate it (variable hoisting, function scope) from other imperative languages, but in the end, most javascript is written as a series of instructions that tell a computer exactly what to do.  While this approach can work, it requires a decent amount of boilerplate removal through standard imperative abstractions (classes, interfaces, etc) that, for the nature of the language it is, requires a fairly deft understanding of object-oriented principles.

Clojure, and in turn ClojureScript, are functional programming languages, which specify a set of transformations on the program state.  Transformations that cause no side effects are known as pure transformations, or pure functions.  Pure functions, because they are deterministic, are referentially transparent, meaning the function invocations can be transparently replaced with the functions output.  This style of writing code is more encouraged in functional paradigms as it makes reasoning about program state much easier.  While this style of writing is possible in imperative languages, it is rarely encouraged, and even less so in Javascript it seems.

An important point to note concerning the difference in philosophy between Clojure and Javascript is that Clojure was the product of one man's vision: Rich Hickey.  ES5/Javascript, by comparison, is a product of several hundreds of individual's inputting ideas.  Design by committee has well known flaws in that it typically lacks vision and cohesive structure.  Strong direction can be a fundamental component to the success of an enterprise as typified by Apple's iPhone (for the first 10 years of its life) as opposed to Android.  While it is true that Android has more market share worldwide, it suffers from lowest-common denominator problems that Apple's approach simply does not.  While the relative merits of one person's vision versus design by committee are debatable, in the case of Clojure, Rich Hickey has provided a very strong and sound foundation for a language that is very opinionated in how it deals with certain problems.

ClojureScript, being an implementation of Clojure that compiles to Javascript as a target, provides several improvements over the standard implementation of ES5 in both syntax and core features.  ClojureScript, like Clojure, is a lisp.  Lisp provides a minimal syntax that allows for the construction of Domain Specific Languages and other advanced metaprogramming techniques that are simple not feasible in unpreprocessed Javascript (and many other languages for that matter).  A minimal list of features that ClojureScript provides over Javascript are: namespaces/module management, multimethods, arity-based dispatch, named parameters, immutable data structures, full fledged maps, sets, sane equality, lambdas, list/map destructuring, dynamic scoping, tail-call optimization, protocols/interfaces, and macros.

Comparative Differences With Current Development
================================================
The current front end development is accomplished by a myriad of tools.  ClojureScript provides a unification and replacement or enhancement for these tools.  Each tool will be described with how it is used, and how a replacement with ClojureScript compares.

<i><span style="color: #999999;">The following toolchain description is indicative of the stack that was used at the time of this writing.</span></i>

Toolchain Comparison
--------------------
Currently for front end development, <i>grunt</i> is the build tool of choice.  It provides a Javascript API and plugins to manage the automated building of Javascript artifacts for deployment.  It currently handles the automation of the following tasks: testing, style enforcement, concatenation, minification, beautification/compression, and source file autodiscovery.

The replacement for this is a Clojure tool known as <i>Leiningen.</i> <i>Leiningen</i>, along with the plugin <i>cljsbuild</i> obviates the need for all of the <i>grunt</i> automation as it stands.  A pure ClojureScript project can be tested with the built in testing facility, it will be compiled and linked properly, and will be compressed into extremely optimized over-the-wire code.  Source file autodiscovery is part and parcel to how <i>Leiningen</i> works and is not a plugin.  The one facility that is not obviated, but which is not as important is style enforcement.  While there is a Clojure style guide, which can be enforced with a plugin known as <i>Kibit</i>, there is much less need for enforcement largely due to the fact that many of the “style” guidelines in <i>JSLint</i>, for example, are based on the idea that changing the style actually leads to hard to find bugs.  This is not as much an issue in a Lisp.

Support Libraries
-----------------
Currently, the preferred programming paradigm for front end development is functional programming.  Javascript has the capacity, but not the built in semantics as part of the core, to facility functional programming.  Because of this, several support libraries are required.  <i>Lo-Dash</i>is a performant and expanded rewrite of <i>Underscore.js</i>, which provides many staples of functional programming, such as map, filter, reduce, partial application, function composition, function currying, and more.  <i>Bilby.js</i> brings the world of strict category theory to Javascript, and in doing so provides multimethod support, monadic constructs, and object lenses.  <i>Ramda</i> is under consideration for addition to this support library because it provides an even more friendly API than <i>Lo-Dash</i> by structuring the functions it provides in a more partial-application or curry-friendly way, thus reducing even more functional boilerplate.

Clojure core library provides everything outlined in all three libraries except one: strict monadic semantics provided by <i>Bilby.js</i>.  The core library provides all the constructs in both <i>Lo-Dash</i> and <i>Ramda</i>, and so obviates the need for both.  It provides multimethods, and functional key-lookups (lens replacement) which mostly obviate the need for <i>Bilby.js</i>.  While not providing strict monadic semantics, several things in Clojure imply monadic semantics.  An example is the thread-first macro which when applied to an object and several key lookups provides an Option/Maybe like experience.

Presentation Libraries
----------------------
Currently, in use presentation libraries include <i>AngularJS</i>, <i>jQuery</i>, and <i>D3</i>.  These are libraries that interface with the DOM and as such are not extrapolations on the linguistic features of Javascript.  There are no direct replacements therefore in ClojureScript, and they would be manipulated directly by the ClojureScript interop features.

Testing
-------
Currently for testing, <i>Jasmine</i>, running on <i>Karma</i>, is what is used to determine correctness for code that is covered.  A wrapper for the <i>Istanbul</i> code coverage tool is used to determine if all code paths and statements are covered.

<i>ClojureScript.test</i> is a port of the standard <i>Clojure.test</i> library for ClojureScript and provides a maximal subset of the semantics of <i>Clojure.test</i>.  Unfortunately, at this time, there is no coverage tool for ClojureScript.

Implementation Considerations
-----------------------------
Given that Javascript is not itself a machine language, and has to be interpreted or compiled before execution, it stands to reason that having Javascript as a target code carries with it some considerations.  Clojure proper compiles down to JVM bytecode which is extremely efficient when running on modern JVM (HotSpot) implementations.  However, Javascript implementations do vary, and so naturally the question arises of using Javascript as an intermediate representation.

ClojureScript addresses this issue in a few ways.  The first is through linkage with the <i>Google Closure Compiler</i>.  ClosureScript is implemented in such a way as to take full advantage of the <i>Google Closure Compiler</i>'s special “Advanced Optimizations” feature, which requires that Javascript be structured in such a way as to allow for proper munging of names as well as several other key features.  Unlike some other implementations of Compile-to-Javascript languages which basically compile their entire runtime, and then bootstrap the bytecode they would normally produce, the usage of the <i>Google Closure Compiler</i>, and specifically its dead or unreachable code removal allows the output from ClosureScript to be much tighter and smaller than other Compile-to-Javascript languages.  The <i>Google Closure Compiler</i> is a very impressive piece of work as far as static analysis of Javascript goes.  The fact that the ClojureScript implementors chose to emit their Javascript in a manner that was compatible with this compilation is a very big boon for reducing the payload hurdle of a Compile-to-Javascript language.

Additionally, a small note should be made that while Javascript supports functional semantics, it is not the most performant code.  Divorcing programming semantics from implementation details has been the point of compilers since they were made, and in this way ClosureScript is no different, allowing for the most performant intermediate code to be produced from the most expressive input.

Finally, it is reasonable to be concerned that if an exception or other problematic issue arises in the usage of Javascript as an intermediate language, it will be hard to resolve the stack trace.  This is not a huge concern because of a modern feature of most Compile-to-Javascript languages called source maps.  Source maps allow a programmer to see where the exception was thrown in the source language, and is used by both people who use Compile-to-Javascript languages and people who just want to compress their code.

Risks
-----
Adoption of a young and somewhat less well known technology is not without its risks.  One of the primary risks that arises specifically with ClojureScript is potentially one with regards to hirability.  Clojure is a functional language, and while functional language adoption is growing, it is not as popular as Object Oriented languages, such as CoffeeScript.  This could lead to the potentiality of a code base that becomes difficult to maintain due to foreignness to future programmers, and ending in a potential worst-case scenario of requiring an entire rewrite.  <i><span style="color: #999999;">Our company, and specifically our business unit</span></i> have had a history of utilizing contractors to fill the talent void.  Contract companies increase the potential pool size to a global audience and mitigate the risk presented to a reasonable degree.

An additional, smaller risk that exists is one that has to do with dealing with a younger technology.  ClojureScript is powerful, but it requires an understanding of JavaScript interoperability and underlying Javascript at a level that may make it difficult for uninitiated contributers to understand or help with, because it is not quite at the auto-magical level that full Clojure is at.  It is compiled to Javascript as an intermediate language, and this means a full and complete understanding of Javascript is required, just as a full understanding of assembly language is required to completely debug a hard to understand bug in a compiled kernel module.

Both of the risks presented follow the same theme that “the talent may just not exist to maintain this.”  The talent to manage these risks currently exists as of this proposal, and the production of product at the hands of inferior tools for the sake of keeping the product alive should not be prioritized over building the product in the first place.  It is easier to maintain what someone has built, than it is to build something new, and it is easier to maintain something with less moving parts than something with more.  ClojureScript means less code, less moving parts, and more robust expressiveness.

Summary
-------
The benefits, comparative analyses, and risks of choosing ClojureScript for future development has been presented in this proposal.  ClojureScript provides a robust framework for the continued development of advanced web applications, providing the proper separation of concerns between maximum expressiveness and client execution considerations, while also providing the ability to develop sanely with testing and debugging processes.  It provides a plethora of linguistic features that outstrip even the most lofty considerations for the ES6/Harmony draft proposal.  ClojureScript will help build a better product, and so therefore it should be employed in future projects and workings in the front-end, and possibly the back end (as ClosureScript or Clojure proper) as the need arises.
