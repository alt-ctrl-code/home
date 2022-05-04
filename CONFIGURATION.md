<a name="logging">Configuration</a>
================================================

This topic considers the notion of application, system or program "configuration", what it is -- _and isn't_ -- and
 explores useful features that might assist us using or deploying our code in a variety of contexts
we might find our code used in.

# Contents
1. [Definition](#definition)
2. [Configuration Options](#options)
   1. [Command Line Arguments](#cli)
   2. [Environment Variables](#env)
   3. [Input / Output](#cli)
3. [A _Thought_ on "Devops"](#devops)
4. [Configuration Or Code](#notseparate)
5. [Outside The Lifecycle](#nothingoutside)
6. [As a "Service"](#notaservice)
7. [Dynamic Configuration](#notdynamic)
8. [A _Thought_ on CONSTANTS & CONFIG](#constants)
9. [A Configuration Façade](#facades)
10. [Feature Wishlist](#features)
    1. [Common File Formats](#formats)
    2. [Polyglot and Isomorphic](#polyiso)
    3. [Comments](#comments)
    4. [Cascading Overrides, with Default Values, Environment Variables and Arguments](#cascades)
    5. [Structured (and Typed) Values](#structured)
    6. [Variable Expansion](#expansion)
    7. [Encrypted Values](#decrypt)
    8. [Remote Values](#fetch)
    9. [Scripting and Expressions](#expressions)
    10. [Integrated with Dependency Injection](#cdi)
    11. [Open For Extension](#open)
11. [Configuration Packages, in The Market](#market)
    1. [Package Feature Matrix](#matrix)
    2. [Buy" vs Build](#buyvsbuild)
    
<a name="definition">Definition</a>
-----------------------------------------------

For our purpose, lets define application configuration as static or immutable contextual information that is
provided on application bootstrap, for the system to behave correctly in a given specific context 
that it is being used.

> __Definition:__ application configuration is static or immutable contextual information that is
>provided on application bootstrap.

By static or immutable, we understand that the configuration and any values or settings specified by it, 
_will not change_ after the system starts.

<a name="options">Configuration Options</a>
-----------------------------------------------

Languages offer various options for injecting application configuration from “outside”.  Server side
langauges have three fundamental ways they expose immediate “system” context information passed into the runtime :

- command-line arguments,
- environment variables, and
- I/O operations.

JavaScript in the browser is unique in this respect and provides the window and window.navigator objects, with
relevant (though not standard) system information.

Let's address server languages here, and defer the browser for later.

### <a name="cli">Command-Line Arguments</a>

Language runtimes provide ways of accessing commandline arguments, traditionally accessed though a `main` function
as an array of string, like `Main (string[] args)` or `main (String[] args)`.

Many ecosystems offer alternatives to the traditional program entry point, for example
.NET also provides `Environment.GetCommandLineArgs();`, and Go provides `os.Args`.

Java retains only the `main (String[] args)`, but the ecosystem (including Spring Boot) tends to use -D and system
properties, for example `-Dspring-boot.run.arguments=--spring.main.banner-mode=off,--customArgument=custom`.

Javascript has the process object, which exposes the `process.argv` and `process.execArgv` arrays with the command-line parameters
provided, including the executable, the target script and then trailing parameters as so.

```shell
// contents of argv.js script console.log(process.argv);
$ node argv.js one two three four five
[ 'node', '/usr/src/argvdemo/argv.js', 'one', 'two', 'three', 'four', 'five' ]
```

Outside of a dedicated command line tools and utilities, it’s a healthy choice to mostly avoid injecting
command line arguments as application configuration.  Any non-trivial system will have a large number of configuration
items, and managing and parsing large numbers of command line arguments quickly becomes unwieldy, and are generally
hard to _source control_

> __Healthy Choice:__  mostly, avoid injecting command line arguments as application configuration.

### <a name="env">Environment Variables</a>

Language runtimes also provide ways of accessing environment variables. .NET provides `Environment.GetEnvironmentVariables();`, Java 
provides `System.getenv`, Javascript `process.env`, and Go provides `os.Environ` or `os.GetEnv`.

With a few notable exceptions, that we will cover soon, it’s a healthy choice to mostly avoid injecting custom
environment variables as application configuration. Again, any non-trivial system will have a large number of configuration
items, and managing and setting large numbers of environment variables quickly becomes unwieldy, and are generally
hard to _source control_.

> __Healthy Choice:__  mostly, avoid injecting environment variables as application configuration.

### <a name="io">Input / Output</a>

Since, as a rule, it’s healthy choice to minimise the use of command line arguments and environment variables,
we are left with the runtime I/O facilities to load application configuration from the file system or network interfaces.

Since for the purpose of this module, we have constrained our definition of application configuration to _static or
immutable_ contextual information provided on application bootstrap, and the _network_ is rarely either of those, it
follows that is generally a healthy choice to get application configuration from the local file system (ideally your
source application bundle)

> __Healthy Choice:__  inject configuration from read-only files bundled with your application.

There are specific exceptions to this choice, for particular reasons that we will cover later.

<a name="devops">A _Thought_ on "Devops"</a>
-----------------------------------------------

Because configuration touches on an important and common organisational and governing "super-structure", we’ll cover it 
now, before we get further into the weeds: it’s a healthy choice to combine development with
operations  – we feel it is the essence of _devops_, more so than merely adopting “infrastructure as code”.  It's what
we might mean if we ever used the phrase “full-stack”, which is a nonsensical term for too many reasons to cover here.

> __Healthy Choice:__  combining development with operations, in a _true_ devops organisational model delivers
> more flexible and scalable applications, more efficiently and more effectively.

Broadly speaking, if your operational environment’s security context is insistent on a “separation of authority” between
development and operations, it is likely to limit your local deployment flexibility, but even so, good application design
can mitigate this, which hopefully we will demonstrate eventually.

<a name="notseparate">Configuration Or Code</a>
-----------------------------------------------

With our opening definition, and in the context of "devops",  beyond what is provided by the language and system runtime 
environment, are the specific configuration items, such as urls, hostnames, ports, accounts and credentials that our 
code requires to run correctly in our various deployment contexts, such as _local, testing, integration, and 
production_, a separate concern to our application code?  

Said another way, should any or all of our code configuration reside somewhere else other than alongside our code? Or is 
it just _more_ code.

From experience, our position is that it is a healthy choice, to maintain as much as possible of our application 
configuration (as we have defined it) alongside our application source code, to version control it _as if it were source
code_ and bundle it along with our application packages.

Said differently, it is a healthy choice to maintain as little context information as possible outside of the
application's own build and deployment lifecycle.

> __Healthy Choice:__  maintain as little context information as possible outside of the
application's own build and deployment lifecycle.

This is the position taken in the Java Spring Boot ecosystem with `application.properties` ,
and also in the .NET Core with `appsettings.json`, where these file collections are generally found (expected) in the project or classpath 
root location.  While in both ecosystems, it is not _mandatory_ to host these configuration resources in the source code
repository, it seems increasingly common to do so. These well established configuration frameworks support it, and it simplifies
our overall development experience and system complexity.

This choice has __important consequences__, because in this position _configuration is code_. Configuration changes 
are code changes, and will be promoted through our build, test and deployment lifecycle. 

Maintaining and deploying configuration, manually or automatically, on a lifecycle outside our application code lifecycle, 
introduces complexity and risk, and reduces the testability and flexibility of our possible deployment configurations, 
especially for local development -- where we need it most.

<a name="nothingoutside">Outside The Lifecycle</a>
------------------------------------------------

If it is a healthy choice to maintain as little context information as possible outside of the application build and
deployment lifecycle, then _what is okay_? 

From experience, it is easiest if the only application configuration  injected into our application from 
outside is the unique handle of the deployment configuration our application will be operating in, and any 
secure secrets required to access sensitive configuration values such as credentials like passwords and tokens.

It is also easier, where practicable and secure, that only a single secure passphrase be injected to 
unlock (decrypt) secret values stored alongside our code.  We will cover a simple approach later.

> __Healthy Choice:__  only a unique handle to the deployment configuration our application will be operating in,
> and a secure passphrase to access sensitive values should be injected from _outside_.

In Java, .NET and JavaScript the use of single deployment context indicators is well-supported.

* In Java and Spring Boot, the unique handle of the deployment configuration is indicated with Spring's active profiles
mechanism, either by supplying the `SPRING_PROFILES_ACTIVE` environment variable (or the `-Dspring.profiles.active` command line 
argument), with comma separated list that dictate the configuration file selection and over-loading order.  

* .NET provides a similar approach, with more "manual" file selection indicated by the developer, using the `FileConfigurationProvider`
([docs](https://docs.microsoft.com/en-us/dotnet/core/extensions/configuration-providers#file-configuration-provider))

* In Node.js,  the [config](https://www.npmjs.com/package/config) package
   supports a familiar Spring Boot-esque application configuration loading, using the __NODE_ENV__ environment 
variable convention .

Support for configuration encrypted at rest in source control is less well-supported over-all, with a lo-fi tendency
to rely on environment variables.

* Spring Boot supports decryption of values stored encrypted in source control of the form `ENC(asdasdasd)` via the Jasypt library and
the `-Djasypt.encryptor.password` argument.

* .NET relies on the use of Secrets Manager, `dotnet user-secrets` command line and associated
loading, requiring the local developer to manually add any required secrets to provide sensitive data, or
write specific configuration loading logic for the local context.

* Other platforms like JavaScript and Go seem not to provide a higher-order mechanism, other than environment variables.

    
Having set the above advice, there may be legitimate non-technical drivers that might preclude maintaining some
configuration values in the code lifecycle.   A system requiring regulatory approvals with digital hashing of the code base, 
where the approval cycle is slower than the volatility of the system configuration (for example, credentials might need
to be refreshed on a schedule faster than the regulator can support or allow), could be such an example.  There would
be endless others.

In general, if necessary,  it's a healthy choice, to minimise non-source controlled configuration as much as possible, and _dereference_ 
those we cannot with a consistent mechanism if possible. 

> __Healthy Choice:__  if necessary, for non-technical reasons, minimise non-source controlled configuration as much as possible, and _dereference_
> those we cannot with a consistent mechanism if possible.


<a name="notaservice">As A Service</a>
------------------------------------------

It is not an uncommon pattern to provide configuration as a service, hosted remotely and ingested over the network. [Spring Cloud Config](https://spring.io/projects/spring-cloud-config) is  
 such an example.

With respect to our __healthy choice__ that it's good to maintain our application configuration 
 alongside our application source code, then it follows that having a build lifecycle for our
configuration in another application service is undesirable. 

It’s generally a healthy choice to __not__ manage your application 
configuration in a separate external (possibly shared) service, as it increases the accidental complexity of the deployment
architecture, making it difficult to configure different and ephemeral deployment options, 
including _local development and testing_.

> __Healthy Choice:__  _do not_ manage our application configuration as a separate external service. Because 
> _accidental complexity_.

Having set the above advice, again where there may be legitimate non-technical drivers that  preclude maintaining some
configuration values in the code lifecycle, then minimise remote configuration values as much as possible, and _dereference_
those we ingest from the remote service with a consistent mechanism if possible.

> __Healthy Choice:__  if necessary, for non-technical reasons, minimise remote configuration values as much as possible, and _dereference_
those we ingest from any remote service with a consistent mechanism if possible.

<a name="notdynamic">Dynamic Configuration</a>
------------------------------------------------------------------

From our definition, we said application configuration is static or immutable contextual information that is provided on
application bootstrap. So it follows that updating system parameters after bootstrap is _no longer the concern of our 
application configuration_. Said differently, configuration is by definition &ndash; _not dynamic_. So it follows 
that it's a healthy choice to separate initial bootstrap configuration from any runtime updates to dynamic application 
switches and routing.

> __Healthy Choice:__  separate initial bootstrap configuration from runtime updates to dynamic application service 
> switches or routing.

More generally speaking, it is also a good pattern to fetch and set initial application parameter values only one-time on startup
from the application configuration, rather than in-line fetching from the config each time they are used. 

> __Healthy Choice:__  fetch and set initial application parameter values from the application configuration one-time, 
> rather than inline fetching from the config each and every time.

Note, a fetch and set pattern (as described above) does not stop us achieving those 
dynamic updates, it is just _not the concern of our application configuration_.  

Polling Launch Darkly, or some other configuration service, or exposing an API interface to tweak a system parameter are user stories with complex 
application integration patterns, all of which increase the entropy of our application architecture, and the 
complexity of our operating processes.

They sure are cool and super-interesting concerns, but we'll park them for later.

<a name="constants">A _Thought_ on CONSTANTS & CONFIG</a>
---------------------------------------------

Before we get further into the weeds with modules, lets have a _think_ on `CONSTANTS`. Configuration and constants
are also separate concerns, since configuration is by definition not CONSTANT.

But it's not uncommon to see something like the following:

```javascript
const CONFIG = {};
CONFIG.ENV = process.env.NODE_ENV; // is a smell
```

We've used JavaScript in the example, noting that JavaScript doesn't cater well for object constants and we adopt `UPPER_SNAKE_CASE` as a well understood
convention to indicate constancy instead, but the smell appears in other ecosystems.

So apart from the obvious, that the attributes of the `CONFIG` const are not in fact constant,
the `NODE_ENV` environment variable is by name and definition a _variable_, consistent only in the lifetime or scope of an
application's deployment context.  This is true of all configuration.

> __Healthy Choice:__  separate CONSTANTS from configuration, especially environment variables.


The `CONFIG` const is also _global_ to the scope of the module, and often seen exported as such. Global config constants
are a smell, and constants should be grouped, enumerated and exposed via a meaningful module or package:

```javascript
const {FINE_STRUCTURE} = require ('FundamentalConstants'); 
const {c,G} = require ('PhysicalConstants');
```
> __Healthy Choice:__  constants should be grouped, enumerated and exposed via a meaningful module or package.


<a name="facades">A Configuration Façade</a>
------------------------------------------

In Java and .NET it is common to adopt the idiomatic configuration framework, for example Spring Boot's properties, or
Microsoft's Configuration extensions.  Other languages, like Node and Go have community packages with similar features, like
npm [config](https://www.npmjs.com/package/config), [gonfig](https://github.com/tkanos/gonfig) or [viper](https://github.com/spf13/viper).

In all cases, it's is a healthy choice it to abstract _all_ our configuration behind a common façade, which has the benefit 
of standardising how it is achieved and encapsulating useful features, including access to environment variables and 
command line arguments.

> __Healthy Choice:__ abstract all config loading behind a common façade, including access to environment variables and
command line arguments.

Generally speaking, where a configuration façade meets our needs, it's a healthy choice to adopt what the ecosystem community uses,
as it simplifies up-skilling new team members, if our solution is _idiomatic_.  

> __Healthy Choice:__ for easier on-boarding, use what the community use.

<a name="features">Feature Wishlist</a>
---------------------------------------------

That said, not all configuration façades are equal, so we'll examine the kind of features that we might want from a configuration
façade package, before we choose, extend or build one from scratch.

### <a name="formats">Common File Formats</a>

It is useful, to be able to specify configuration in a variety of familiar (and perhaps less familiar) formats - especially
the most common ones (YAML, JSON, Java Properties).  TOML, HCL and others are interesting, but perhaps more idiosyncratic. 

> __Healthy Choice:__ we accept it is a "highly personal" preference, but yaml provides a good balance of fluency, nesting
> and in-line commentary

XML is increasingly rare, likely because of its unwieldy syntax, and essential complexity (attributes, schemas etc).  It's
a healthy choice to avoid the use of XML.  Thank-you Douglas Crockford (for JSON), and Clark Evans et al (for YAML).

> __Healthy Choice:__ avoid xml, it was bad for parsers, and worse for humans.  

### <a name="polyiso">Polyglot and Isomorphic</a>

It is useful, to be able to use the same configuration mechanism in various languages (polyglot), particularly in the browser,
and even to share the same code between server and client or browser (isomorphic).


### <a name="comments">Comments</a>

It is useful, as a consequence of the file format we choose, to have comments in our configuration files.  YAML and Java
property files support comments, whereas JSON typically does not.

### <a name="structured">Structured (Typed) Values</a>

Most config packages support a hierarchical (dot separated) format for configuration values, and it is a healthy choice
to always scope your configuration values to avoid naming collisions and conflicts, when applying modular application 
architectures.

> __Healthy Choice:__ scope your configuration values to avoid naming collisions and conflicts, with other packages

For example, scoping log related configuration under the “logging” configuration path, and is a good example of
how this rule is applied.

```yaml
logging :
  #set a format to json for JSON output
  format : json
  #set a level for logging categories
  level:
    # root log level
    "/" : error
```

It is sensible to scope all our config under the name of our package or module.  

> __Health Choice:__ scope your configuration values under the name of our package or module.

In strongly typed languages, like Go and C#, configuration packages also allow loading of structured configuration into
classes or structs, which can be beneficial in the context of those languages.

### <a name="cascading">Cascading Overrides, with Default Values, Environment Variables and Arguments</a>

Adopting a configuration façade, allows us to intercept values provided as environment variables, set context-aware values, 
for example between test and production environments, and to provide default values for those when they are not provided
by the runtime system, for example in the local development context.

`local-development.yaml`
```yaml
env:
  A_PROVIDED_VAR: 'noneedtosetthislocally'
```

It's a healthy choice to use context-aware configuration façade, to avoid any manual set up or intervention in local-development.  This
helps new team members stream-line the project setup locally, if we do not have to fiddle with settings outside of the project.

> __Healthy choice:__  avoid any manual set up or intervention especially in local-development.

### <a name="expansion">Variable Expansion (Placeholder Resolution)</a>

A feature of some configuration libraries is variable expansion, which supports modularity and re-use across
configuration keys, but many do not support it.

Here is an example:

```yaml
ourapp:
    protocol: "https"
    host: "myserver.com"
    port : "8080"
    context :
    url : "${protocol}://${host}:${port}/${context}",
    endpoints : 
      customer : "${url}/customer"
      account : "${url}/account"
```

> __Healthy choice:__  defer complicated configuration expressions to initialisation code in the application itself.

### <a name="decrypt">Encrypted Values</a>

Another common utility feature of configuration libraries is decrypting config values encrypted with a passphrase
maintained outside of the source repository, effectively obfuscating your secret values as below.

```yaml
ourapp:
    credentials:
        user: "svc.account"
        pass: "enc.pxQ6z9s/LRpGB+4ddJ8bsq8RqELmhVU2"
```

It’s a healthy choice, in controlled development zones, to use encrypted properties as an effective strategy to have
our secret keys maintained as source code, and not injected or maintained on a separate deployment lifecycle,
increasing our deployment complexity.

> __Healthy Choice:__ use encrypted properties as an effective strategy to have our secret keys maintained
> as source code.

### <a name="fetch">Remote Values</a>

In those use cases, examined earlier, where bundled static configuration values may be problematic, it is useful to have
a consistent approach to referencing values stored in a remote service, as below:

```yaml
ourapp:
    remote : 
        url: "${config.server}/${env.NODE_ENV}/myapp?path=myapp.volatiles.somevalue",
        method: get,
        body: {},
        headers:
          authorization: "enc.pxQ6z9s/LRpGB+4ddJ8bsq8RqELmhVU2"
          Content-Type: "application/json"
```

### <a name="expressions">Scripting and Expressions</a>

Some configuration libraries such Spring (SpEL), and Hashicorp's HCL offer support for more complex expression resolution,
in the configuration loading (or injection).

Beyond the use cases we have set out above, which support modularity (re-use), source-controlled secrets, and de-referencing
volatile values, it's a healthy choice to avoid complex expressions in configuration, and defer them to initialisation code
within the application itself.

> __Healthy Choice:__ avoid complex expressions in configuration, and defer them to initialisation code
within the application itself.

### <a name="cdi">Integrated with Dependency Injection</a>

It can be useful,  if we prefer to use a dependency injection mechanism, if the configuration façade is integrated with
the DI mechanism, allowing class or structure properties to be wired directly from the configuration, for example Spring Boot has a
well understood mechanism for this via its @Value annotation.

### <a name="open">Open For Extension</a>

Ideally, whichever mechanism we choose, it may never fit the unique needs of our application, and those cases it is useful if the
configuration façade is "open for extension", with a pluggable design that can supports novel behaviours not catered for by the original
authors.

<a name="facades">Configuration Packages, in The Market</a>
------------------------------------------

We have mentioned several configuration packages throughout the text, and we compare them below.

### <a name="matrix">Package Feature Matrix</a>

### <a name="buyvsbuild">"Buy" vs Build</a>