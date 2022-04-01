<a name="welcom">Welcome to @alt-javascript</a>
=============================

<a name="jsdnd">JavaScript Design and Development</a>
-----------------------------------------------------

A guided design and development exercise in _plain old_ JavaScript.
# Contents
1. [Introduction](#intro)
1. [Audience](#audience)
2. [Some History](#history)
3. [Why The History Matters](#javascriptmatters)
4. [Structure](#structure)
    1. [Module 1 Package Hygiene](https://github.com/craigparra/alt-package-conventions)
    1. [Module 2 Logging](./LOGGING.md)
    1. [Module 3 Configuration](./CONFIGURATION.md)

<a name="intro">Introduction</a>
--------------------------------

It's easy to think of the internet as not being grounded anywhere, but nothing is performed 
outside of its a cultural and political context, and software design is no exception, so we offer you the following
two expectation setting sections, before we get into the main part of the material.

We live and work in Australia, where "down under", it’s common and right to lead with an acknowledgement of country.

### Acknowledgement of Country

We acknowledge and pay respect to the Elders past and present, to those
who have passed before us and to the members of the Aboriginal and Torres Strait Islander
community. We acknowledge the Turrbal and Yuggera Peoples, the traditional custodians of the beautiful city we live, 
work and prepare this material in.

### A Safe Space

We also acknowledge that discrimination and harassment persist and pervade, and that the software community continues to be more
difficult and inequitable for women, for people of colour, for the LGBTQI+ community, for those to whom English is a 
Second Language ([ESL](https://simple.wikipedia.org/wiki/English_as_a_second_language)), and for our "offshore" cohorts.

I check my privilege in the community as an ("onshore", monolingual) older, white, cisgender male, and work to balance the scale.
In the spirit on inclusion, all are safe and welcome here, my preferred pronouns are his/him, but I otherwise adopt we/us 
throughout the material. If you'd like to know more about me [IRL](https://simple.wikipedia.org/wiki/Internet_slang#:~:text=Some%20existing%20acronyms%2C%20such%20as,before%20the%20internet%20became%20popular.)
, I  link a short [bio](./BIO.md) 

With all that said, we hope that you enjoy and find value in the material.

<a name="audience">Audience</a>
-------------------------------
The @alt-javascript material is meant for JavaScript developers of all levels, junior and “senior”, fresh and 
not-so-fresh.   It assumes at least some level of familiarity with node.js and the developer stack such as mocha, 
eslint and nyc, and though it doesn't cover using those tools directly, the material does refer to and include them from
time to time.   

The material focuses on good application design, which is higher-order conceptually than the tools you use, or whether 
you adopt TDD, linting and so on, and the concepts escalate quickly.  Suspend your disbelief, and who knows, you may 
learn a trick or two.

> A word on __TypeScript__ : this material is deliberately about JavaScript.
>
> At best, TypeScript should be thought of as a bridging technology for C# and Java developers.  At worst, 
> another sophisticated attempt by Microsoft to colonise the internet, _along with vscode_
> (we don't need a [VBScript 2.0](https://en.wikipedia.org/wiki/VBScript))
> 
> TypeScript is a superset of JavaScript, meaning all the devils are lurking there &ndash; no amount of static typing 
> can save you from yourself, and besides, if you are using eslint, mocha and nyc _as you should be_, then your 
> JavaScript is safe, and your tests will express your design intent stronger than types.
>
> I'm not saying don’t use TypeScript, if you must, only that the choice to use it is as political as it is technical, 
> and we choose JavaScript &ndash; you don't need TypeScript, and learning in the internet's first
> language will make your TypeScript all the better.
>

<a name="history">Some History</a>
----------------------------------------

The ascent of the world wide web after Tim Berners Lee wrote the first hyper text (HTML) browser, literally called
[WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb) in __1990__, was both culturally and technically transformative. 
It’s not hyperbole to suggest software development changed dramatically and forever in that year, some __30__ years ago.

Marc Andreesen released the first multimedia browser [Mosaic](https://en.wikipedia.org/wiki/Mosaic_(web_browser))
browser in __1993__, and founded [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape_Navigator) in __1994__. Brendan 
Eich was tasked with extending Netscape with a scripting language, and the company shipped JavaScript in __1995__.
      
JavaScript was syntactically similar to, but definitely NOT like, Java the “hot new” programming language also
released in __1995__, by James Gosling at Sun Microsystems.   Ten years of horse crazy browser side JavaScript ensue, 
matched only by the ten equal years of bat crazy server side Java and .NET. Browser wars erupt, Sun invents J2EE 
(now just EE), Weblogic usurps the “application server”,  Microsoft releases Active this and that, IBM kind of 
gets involved.
  
Then in __2006__, AJAX (Asynchronous JavaScript and XML oh, that) is standardised, and now JavaScript can
remote. Browser side scripting madness erupts all over again with binding and remoting.

Three years later in __2009__, Ryan Dahl remixes Google Chrome’s [V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) 
JavaScript interpreter with an event loop and I/O API to make Node.js, and isomorphic JavaScript applications and 
the [MEAN](https://en.wikipedia.org/wiki/MEAN_(solution_stack)) stack go mainstream.

Until finally JavaScript eats the world, and is arguably now the de facto  lingua franca of software development.

<a name="javascriptmatters">Why The History Matters</a>
----------------------------------------

With its slacker origins in the nineties, JavaScript has always and unfairly been seen as the awkward - even dangerous -
misfit of software development.  So much so, that the inventor of JSON, Douglas Crockford declared it:

>  The world’s most misunderstood programming language.


But it was _deliberately_ light weight, flexible _and powerful_, now so more than ever.

But many developers, and many _common_ packages  elide the design rigour seen in similar code in Java and C#. It 
has taken over a decade for native JavaScript frameworks to mature. Ten years later than the same lessons in server-side
Java and C#.  Much of what you might find in the JavaScript ecosystem today, is _“politely”_ somewhere to the right of where it
needs to be (assuming left is good, right?).

This material applies common programming and architecture patterns, to make your JavaScript better &ndash; because when
used right, JavaScript is awesome.

<a name="structure">Structure</a>
-------------------------------
The material is built up over a series of modules. Each module contributes toward a small but complete progressive web 
application with supporting API, SDK and CLI, running natively in the cloud. The application is a very narrow slice, 
allowing a razor focus on the specific design and code hygiene elements, without the burden of verbose application 
features and code.

The module application source and packages are provided (and linked) publicly here on [GitHub](https://github.com/) and
[npm](https://www.npmjs.com/) .

- [Module 1 Packaging](https://github.com/craigparra/alt-package-conventions#readme)
- [Module 2 Logging](./LOGGING.md)
- [Module 3 Configuration](./CONFIGURATION.md)
