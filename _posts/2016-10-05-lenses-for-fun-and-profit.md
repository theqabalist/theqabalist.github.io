---
layout: post
title: The Case For Lenses in Pragmatic Business Applications
date: '2016-10-05T20:14:00.000-05:00'
author: Brandon
tags:
modified_time: '2016-10-06T08:14:35.307-05:00'
blogger_id: tag:blogger.com,1999:blog-6424479588103664299.post-6669772652598326233
blogger_orig_url: http://www.brandonkeown.com/2016/10/lenses-for-fun-and-profit.html
---

So it occurred to me today, as I was explaining an example of how to use [Ramda Lenses](http://ramdajs.com/docs/#lens) that the value that lenses provide is something that is simple, but not so immediately obvious. A lot of people do not understand why you would use lenses in a business application, and I have had trouble explaining why you might want to in my own dealings with other coworkers, over something like plain addressing or encapsulation in an object.

Put succinctly, lenses are a concept that abstract away data structure addressing, access, and modification. This is dry, so I will attempt to put it in pragmatic terms. The power that lenses bring to a project is the ability to refer to addressing logic in a generic way that allows for the replacement of the underlying data structure without modification to any consumer code.

An example is in order.

Let's say you have a person represented in a fairly obscure structure, maybe a list:
<pre><code class="javascript">const personList = ["John Smith", 25, "123 Flower Ln", "Dallas", "Texas", "75204"];</code></pre>

And now you want to get the state, well that's easy, just reach for the 4th element:
<pre><code class="javascript">console.log(`proc list: ${personList[4]}`);</code></pre>

Now your boss comes to you and says, good news! We can represent a person as an object now!

You look over the specification of person now, and it looks more like the following:
<pre><code class="javascript">const personObject = {
  name: "John Smith",
  age: 25,
  address: {
    street: "123 Flower Ln",
    city: "Dallas",
    state: "Texas",
    zip: "75204"
  }
}</code></pre>

So you find and replace all instances of your list index with the following:
<pre><code class="javascript">console.log(`proc object: ${personObject.address.state}`);</code></pre>

A few months pass, the team has been infiltrated by elitist functional purists who insist on using immutable structures; or maybe they just want full blown maps in ES5...who knows. &nbsp;Your boss now apprises you to the fact that the person objects look like this:
<pre><code class="javascript">const personMap = Immutable.Map()
  .set("name", "John Smith")
  .set("age", 25)
  .set("address", Immutable.Map()
        .set("street", "123 Flower Ln")
        .set("city", "Dallas")
        .set("state", "Texas")
        .set("zip", "75204"))</code></pre>

So you once again try to find and replace all your instances like the following:
<pre><code class="javascript">console.log(`proc map: ${personMap.get("address").get("state")}`);</code></pre>

Exhausted from the change, and irritated at the edge cases where you missed a few consumers, you set out to refactor the code.  Trusty object encapsulation to the rescue:
<pre><code class="javascript">////// object approach

//// person encapsulating list
class Person {
  constructor(person) {
    this.person = person;
  }

  state() {
    return this.person[4];
  }
}

// modified to

//// person encapsulating object
class Person {
  constructor(person) {
    this.person = person;
  }

  state() {
    return this.person.address.state;
  }
}

// modified to

//// person encapsulating immutable map
class Person {
  constructor(person) {
    this.person = person;
  }

  state() {
    return this.person.get("address").get("state");
  }
}</code></pre>

Now your consumer code looks uniform:
<pre><code class="javascript">console.log(new Person(underlying).state());</code></pre>

But did you need to immediately jump to encapsulation?  Could you be introducing incidental complexity by using objects over the underlying data structures? Potentially, when it comes to mutation especially, objects can hide mutation, and you know your elitist functional overlords will never go for that...what to do?

This is where lenses come into play. &nbsp;Lets take a look at the two lenses that have helpers first:
<pre><code class="javascript">////// function lens approach

//// index lens
const stateLens = R.lensIndex(4);
const person = personList;

// modified to

//// object lens
const stateLens = R.lensPath(["address", "state"])
const person = personObject;</code></pre>

But what do we do for the case when we want to use a more exotic underlying structure? We construct our own lens:
<pre>
<code class="javascript">//// exotic lens
const immutableGet = R.invoker(1, 'get');
const immutableSet = R.invoker(2, 'set');
const immutableAddressLens = R.lens(immutableGet('address'), immutableSet('address'));
const immutableStateLens = R.lens(immutableGet('state'), immutableSet('state'));
const stateLens = R.compose(immutableAddressLens, immutableStateLens);
const person = personMap;</code></pre>

And now our consumer code looks uniform:
<pre><code class="javascript">console.log(R.view(stateLens, person));</code></pre>

Both objects and lenses provide the ability to abstract over data access.  Unlike objects, lenses come with three useful functions: view (allowing you to see the value), set (allowing you to change the value immutably), and over (view, apply function, set). &nbsp;These three operations are most of the boilerplate that is abstracted over in classical objects, but you can get them for free with lenses.

Additionally, if you'll notice, the lenses were built piecemeal and then composed together using regular function composition.  This is an important additional benefit.  Lenses (of the type in Ramda), are just functions, and they compose nicely with any other functions.  The lensPath helper in the previous block abstracts over even this aspect.  It could have been constructed using composition and Ramda's lensProp function.

To revisit -- lenses abstract data addressing, access, and modification in a uniform (non-idiosyncratic or boilerplate) way providing uniform consumer code and the ability to change the underlying structure without modifying consumer code, without the headache or boilerplate of classes.

<a href="http://jsbin.com/nolitajaki/edit?html,js,console" target="_blank">All functioning code in JSBin</a>
