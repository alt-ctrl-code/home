<a name="welcom">Welcome to @alt-javascript</a>
=============================

<a name="jsdnd">JavaScript Design and Development</a>
-----------------------------------------------------
A guided design and development exercise in _plain old_ JavaScript
# Contents
1. [Audience](#audience)
2. [Some History](#history)
3. [Why The History Matters](#javascriptmatters)
4. [Structure](#structure)
    1. Module 1 Package Hygience
    1. [Module 2 Logging](./LOGGING.md)
    1. Module 3 Configuration

<a name="audience">Audience</a>
-------------------------------
The @alt-javascript material is meant for  JavaScript developers of all levels, junior and “senior”, fresh and 
not-so-fresh.   It assumes at least some level of familiarity with node.js and the developer stack such as mocha, 
eslint and nyc, and though it doesn't cover using those tools directly, the material does refer to and include them from
time to time.   

The material focuses on good application design, which is higher-order conceptually than the tools you use, or whether 
you adopt TDD, linting and so on, and the concepts escalate quickly.  Suspend your disbelief, and who knows, you may 
learn a trick or two.

> A word on __TypeScript__ : this material is deliberately about JavaScript.
> TypeScript is a superset of JavaScript, meaning all the devils are lurking there &ndash; no amount of static typing 
> can save you from yourself, and besides, if you are using eslint, mocha and nyc _as you should be_, then your 
> JavaScript is safe.
>
> We’re not saying don’t use TypeScript, it’s great, only that learning in the lingua franca will make your TypeScript
> all the better.

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
matched only by the ten equal years of bat crazy server side Java and .NET. Browser wars erupt, Weblogic invents the 
“application server”, Sun invents J2EE (now just EE), Microsoft releases Active this and that, _IBM gets involved_.
  
Then in __2006__, AJAX (Asynchronous JavaScript and XML oh, that) is standardised, and now JavaScript can
remote. Browser side scripting madness erupts all over again with binding and remoting.

Three years later in __2009__, Ryan Dahl remixes Google Chrome’s [V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) 
JavaScript interpreter with an event loop and I/O API to make Node.js, and isomorphic JavaScript applications and 
the [MEAN](https://en.wikipedia.org/wiki/MEAN_(solution_stack)) stack go mainstream.

Until finally JavaScript eats the world,and is arguably now the de facto  lingua franca of software development.

<a name="javascriptmatters">Why The History Matters</a>
----------------------------------------

With its slacker, [Gen X](https://en.wikipedia.org/wiki/Generation_X) origins in the nineties, JavaScript has always 
and unfairly been seen as the grunge problem-child of software development. It was deliberately, light weight, flexible 
_and powerful_, now so more than ever.

But many developers, and many _common_ packages  elide the design rigour seen in similar code in Java and C#. It 
has taken over a decade for native JavaScript frameworks to mature. Ten years later than the same lessons in server-side
Java and C#.  Much of what you might find in the JavaScript ecosystem today, is _“politely”_ somewhere left of where it
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

- Module 1 Packaging
- [Module 2 Logging](./LOGGING.md)
- Module 3 Configuration