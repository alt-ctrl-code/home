<a name="welcom">Welcome to `alt-ctrl-code`</a>
=============================

<a name="dnd">Software Design and Development</a>
-----------------------------------------------------

A language agnostic meditation on software design and explorations in code.

# Contents
1. [Preamble](#preamble)
1. [Audience](#audience)
2. [Some History](#history)
3. [Why The History Matters](#javascriptmatters)
4. [Approach](#approach)
    1. [Module 1 Package Hygiene](https://github.com/craigparra/alt-package-conventions)
    1. [Module 2 Logging](./LOGGING.md)
    1. [Module 3 Configuration](./CONFIGURATION.md)

<a name="preamble">Preamble</a>
--------------------------------

It's easy to think of the internet not _grounded_ in any place. But, nothing is performed 
out of its a cultural context, and coding is no exception, so we offer you this preamble with
two small sections, to set expectations, before we get into the rest of the site. In Australia, where 
we live, it’s appropriate to lead with an acknowledgement of country.

### Acknowledgement of Country

We acknowledge and pay respect to the Elders past and present, to those
who have passed before us and to the members of the Aboriginal and Torres Strait Islander
community. We acknowledge the Turrbal and Yuggera Peoples, the traditional custodians of the city we live and
work in.

### A Safe Space

We acknowledge that discrimination exists, and that the coding community is more
difficult and more inequitable for women, for people of colour, for LGBTQI+ individuals, and for those whose English is a 
Second Language ([ESL](https://simple.wikipedia.org/wiki/English_as_a_second_language)). I recognise my privilege and hope to balance the scale. In the spirit on inclusion, all are safe and welcome at
`alt-ctrl-code`, my preferred pronouns are his/him, but I otherwise adopt we/us on the site.  If you'd like to know more about me [IRL](https://simple.wikipedia.org/wiki/Internet_slang#:~:text=Some%20existing%20acronyms%2C%20such%20as,before%20the%20internet%20became%20popular.)
, I  link a short [bio](./BIO.md).  With all that said, we hope that you enjoy and find value in the work.

<a name="audience">Audience</a>
-------------------------------
Our goal at `alt-ctrl-code` is sharing with coders at any level, junior and “senior”, fresh or not-so-fresh.   We assume some familiarity with coding in
a developer stack, perhaps JavaScript or Go or Python, and though we don't aim to cover languages directly, we do refer to and include them from
time to time.   This is our extended meditation on application design, regardless of the particular stacks we use, or whether 
we adopt TDD, linting and so on.  The explorations escalate naturally, so suspend your disbelief, and enjoy the show.  

We hope you find a flavour to like or two.


<a name="history">Some History</a>
----------------------------------------

From the advent of _modern_ programming languages with __FORTAN, Lisp and COBOL__ emerging in the late 1950's, though __ANSI C__
in the early 1970's and into the popularisation of objected-oriented languages like Smalltalk, Objective C and C++ in the 1980's, 
_software engineering_ was for the most part a stable affair.  We might have been a COBOL or C prog for our entire career, give or
take. But then -- 

The invention of the world wide web after Tim Berners Lee wrote the first hyper text (HTML) browser, literally called
[WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb) in __1990__, was transformative. 
It’s not hyperbole to suggest software development began to change dramatically that year, __30__ odd years ago.
Netscape shipped __JavaScript__ in __1995__, the same year that gave us __Java, Ruby, PHP__ and the __Perl__ [CPAN](https://en.wikipedia.org/wiki/CPAN) -
getting dynamic web development going.  The noughties gave us __C#__ (2000), then __Scala__ (2004), __AJAX & Web 2.0__ (2006),
__Groovy__ & __Clojure__ (2007), __Node.js__ (2009), and in just the last decade __Go__ (2012), __Swift__ (2014), __Rust__ (2015) and __Kotlin__ (2016).

It's non-exhaustive, but still a dizzying cosmos of languages, ecosystems and communities to navigate for veteran and rookie alike.   Whether we cut our geek 
on Java or C#, or got started in Ruby or Node or whatever, the diversity of our industry is crazy.


<a name="javascriptmatters">Why The History Matters</a>
----------------------------------------

While all languages and different, in that they emerge at different times with different goals, and may have radically 
divergant idioms and norms, whether its the enterprise "safety" of Java and C#, the isomorphism of JavaScript ("The world’s most misunderstood programming language"), the pure
productivity of Python, or the frugal asceticism of Go,  whatever attracts us, the goals of the coder more often remain doggedly consistent, whether parsing a file or 
exposing an endpoint, and sampling a generous variety is probably healthy advice.


Exploring design approaches and coding choices, across ecosystems, makes our work better &ndash; because when we lean into "learning", code is fun and awesome.

<a name="approach">Approach</a>
-------------------------------
Our goal is to build over a series of modules. Each module advocates toward a small but "complete" progressive web 
application with supporting API, SDK and CLI, running in the cloud. Our aim is to "think out" a very narrow slice, 
allowing us to focus on the specific design and code  elements, without the cognitive burden of busy application 
features and code.

Module application example source and packages are provided (and linked) publicly here on [GitHub](https://github.com/) and
[npm](https://www.npmjs.com/) .

- [Module 1 Packaging](https://github.com/craigparra/alt-package-conventions#readme)
- [Module 2 Configuration](./CONFIGURATION.md)
- [Module 3 Logging](./LOGGING.md)

