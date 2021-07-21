<a name="logging">Configuration Design Trail</a>
=============================

This material extends the excellent [config](https://www.npmjs.com/package/config) package, 
adding some utility features with a simple wrapping facade, guided by good design rules and architecture principles.

<a name="notseparate">Not A Separate Concern</a>
-----------------------------------------------

For our purpose, we are going to define of application configuration as static or immutable contextual information that is 
provided on application bootstrap.

> __Definition:__ application configuration is static or immutable contextual information that is
>provided on application bootstrap.

With that, beyond what is provided by the browser or node process, are the urls, hosts, ports, accounts and credentials 
that our application requires in various deployment configurations such as _local, dev, uat, and production_, a separate
concern to our application code?  To expand on that, should any or all of application configuration reside somewhere 
else other than alongside the application source code?

This is an _opinionated_ position, but it is a good design rule, and as much as possible, to maintain your application 
configuration (as we have defined it) alongside your application source code, to version control it _as if it were source
code_ and bundle it along with your application packages.

Said differently, it is a good design rule to maintain as little context information as possible outside of the 
application's own build and deployment lifecycle.

> __Good Design Rule:__  maintain as little context information as possible outside of the
application's own build and deployment lifecycle.

This has __deep and important consequences__, because in this position _configuration is code_. Configuration changes are 
code changes, and should be promoted through the build, test and deployment cycle. Maintaining and deploying 
configuration, either manually or automatically, on a lifecycle outside of your application code lifecycle, 
introduces complexity and risk and reduces the testability and flexibility of your possible deployment configurations, 
especially to the local developer (who needs it the most).


<a name="nothingoutside">Nothing Left Outside</a>
------------------------------------------------

If it is a good design rule to maintain as little context information as possible outside of the application build and
deployment lifecycle, then _what is okay_? 

It is a good design rule that the only application configuration that should be injected into your application from 
outside should be the unique handle of the deployment configuration your application will be operating in, and any 
secure secrets required to access sensitive configuration values such as credentials like passwords and tokens.

It is also a good design rule, that where practicable and secure, only a single secure passphrase should be injected to 
unlock (decrypt) secret values stored alongside your code.  There are various approaches to managing secret 
configuration, with increasing sophistication, but we will include a simple approach later in this material.

> __Good Design Rule:__  only a unique handle to the deployment configuration your application will be operating in,
> and a secure passphrase to access sensitive values should be injected from _outside_.  That's right, just two.

In Node.js, it’s a good design rule to use the __NODE_ENV__ environment variable convention to inject your unique handle 
of the deployment configuration, and use production as the conventional handle for “live” (there are solutions for 
scaling also).  We will not cover config in the browser in this material, but later.

> __Good Design Rule:__  use the NODE_ENV environment variable convention to inject your unique handle
> of the deployment configuration, and use production as the conventional handle for “live”.

<a name="notaservice">Not As A Service</a>
------------------------------------------

It is not an uncommon pattern to provide configuration as a service, hosted remotely and ingested over the network.

With respect to our __opinionated position__ that its good to maintain your application configuration 
(as we have defined it) alongside your application source code, then it follows that having a build lifecycle for your
configuration in another application service is bad. It’s a good design rule to __not__ manage your application 
configuration in a separate external (possibly shared) service, as it increases the accidental complexity of the deployment
architecture, making it difficult to configure different and ephemeral deployment options, 
including _local development and testing_.

> __Good Design Rule:__  _do not_ manage your application configuration as a separate external service. Because 
> _accidental complexity_.

<a name="not dynamic">Dynamic Configuration, Is No Such _Thing_</a>
------------------------------------------------------------------

From our definition, we said application configuration is static or immutable contextual information that is provided on
application bootstrap. So it follows that updating system parameters after bootstrap is _no longer the concern of your 
application configuration design_. Said differently, configuration is by definition &ndash; _not dynamic_. So it follows 
that it's a good design rule to separate initial bootstrap configuration from any runtime updates to dynamic application service flags and parameters.

> __Good Design Rule:__  separate initial bootstrap configuration from runtime updates to dynamic application service 
> flags or parameters.

A good pattern is to fetch and set initial application service  parameter values from the application configuration 
facade, rather than fetching directly from the config each time. The use of the 
[LoggerRegistry](https://github.com/craigparra/alt-logger/blob/master/LoggerRegistry.js) by the `LoggerFactory` in the
[Logging](./LOGGING.md) trail is a good example of this separation; if the `ConfigurableLogger` were to rely on the 
immutable configuration values, we would not be able to alter our logger category levels at run time, which we 
definitely want to do.

> __Good Design Rule:__  fetch and set initial application service parameter values from the application configuration facade, 
> rather than fetching directly from the facade each and every time.

Note, and this is _important_, a fetch and set pattern (as described above) does not dictate _how_ you achieve those 
dynamic updates, and it is _not the concern of your application configuration design_.  Polling Launch Darkly, or some 
other configuration service, or exposing an API interface to tweak a system parameter are user stories with complex 
application integration patterns, all of which increase the entropy of your application architecture, and the 
complexity of your human operational interface (e.g., where do I go to increase logs level again?).

They sure are cool and super-interesting concerns, but let's park them for later.

<a name="jscondig">JavaScript Configuration</a>
-----------------------------------------------

JavaScript offers various options for injecting application configuration from “outside”. Browsers provide the 
window and window.navigator objects, with relevant (though not standard) system information, and Node.js has three 
fundamental ways it exposes immediate “system” context information passed into the runtime interpreter: 

- command-line arguments,
- environment variables, and 
- I/O operations.

We'll  address node.js here, and defer the browser for later.

### command-line arguments

The node runtime provides the process object, which exposes the `process.argv` array with the command-line parameters 
provided, including the executable, the target script and then trailing parameters as so.

```shell
// contents of argv.js script console.log(process.argv);
$ node argv.js one two three four five
[ 'node', '/usr/src/argvdemo/argv.js', 'one', 'two', 'three', 'four', 'five' ]
```

The process object also exposes the `process.execArgv` array with the Node.js specific command-line parameters provided,
such as –version and the `process.execPath` with the absolute pathname of the executable that started the Node.js process.

Outside of a dedicated command line interface (cli), it’s a good design rule to mostly avoid injecting 
command line arguments as application configuration.

> __Good Design Rule:__  mostly, avoid injecting command line arguments as application configuration.

### Environment Variables

The node runtime process object also exposes the `process.env` object with the “visible” system environment variables, 
such as `USER`, `PATH`, `HOME` and so on. It is also possible to set an environment variable directly on the node 
command line with the –e switch.

```shell
$ node –e ‘process.env.foo = “bar”’ myapp/index.js
```

With a few notable exceptions, that we will cover soon, it’s a good design rule to mostly avoid injecting custom 
environment variables as application configuration.

> __Good Design Rule:__  mostly, avoid injecting environment variables as application configuration.

### Input / Output

Since, as a rule, it’s good design practice to minimise the use of command line arguments and environment variables, 
we are left with the Node.js I/O facilities to load application configuration from the file system or network interfaces.
Since for the purpose of this module, we have constrained our definition of application configuration to _static or 
immutable_ contextual information provided on application bootstrap, and the _network_ is rarely either of those, it 
follows that is is a good design not to get application configuration from the file system (and in fact, your 
application file bundle)

> __Good Design Rule:__  inject configuration from files bundled with your application.

<a name="constants">The Word on CONSTANTS</a>
---------------------------------------------

Now, before we get into the weeds with modules, lets just have _the word_ on `CONSTANTS`. Configuration and constants 
are also separate concerns, since configuration is by definition NOT CONSTANT.  

But it's not uncommon to see this kind of code.

```javascript
const constants = {};
constants.ENV = process.env.NODE_ENV; // is a smell, say no more
```

Apart from the obvious, that the `ENV` property is not in fact a `const` (noting that JavaScript doesn't cater well for 
class constants (yet) and we adopt `UPPER_SNAKE_CASE` as a well understood convention to indicate constancy instead),
the `process.env.NODE_ENV` is by definition, essentially a variable, constant only in the lifetime or scope of an
application's deployment context.

> __Good Design Rule:__  separate CONSTANTS from configuration.


The `constants` const is also _global_ to the scope of the module, and worse, often exported as such. Global constants
are a smell, and constants should be grouped, enumerated and exposed via a meaningful module:

```javascript
const {FINE_STRUCTURE} = require ('FundamentalConstants'); 
const {c,G} = require ('PhysicalConstants');
```
> __Good Design Rule:__  constants should be grouped, enumerated and exposed via a meaningful module.

<a name="jscondig">Common Configuration Modules</a>
-------------------------------------

### dotenv and dotenv-expand

The JavaScript open-source community provides the popular [dotenv](https://www.npmjs.com/package/dotenv) packages for 
locally managing application configuration provided as environment variables. On the basis that the only application configuration that 
should be injected into your application from outside should be the unique handle of the deployment (via NODE_ENV) , 
it follows that it is also a good design rule to aver the need for and use of dotenv and related `.env` files that are 
uncontrolled via `.gitignore`.

We're not saying dotenv isn't great, it is &ndash; just that avoiding environment variables negates the need for it.

> __Good Design Rule:__  aver thus use of dotenv, if you can avoid the use of env vars.

### [node-]config

The JavaScript open-source community also provides the excellent [config](https://www.npmjs.com/package/config) package 
for managing application configuration. As with the [logging](./LOGGING.md) material, it is a good design rule to 
abstract all our configuration behind a common façade, which has the benefit of standardising how it is achieved and 
encapsulating common good design features.

> __Good Design Rule:__ abstract all config loading behind a common façade.

It is a good design rule to use the [config](https://www.npmjs.com/package/config)  package façade to isolate all 
configuration, and aver the use of `.env` files in favour of a `config/local-development.json` file. It is also good 
design rule to use JSON as the format, rather than .js or .yml (yes, yaml). Executable configuration and deferred evaluation at bootstrap are just bad in principle, and we don’t need yet another lazy markup syntax, especially one that uses whitespace as a scope discriminator.

> __Good Design Rule:__ prefer a [config](https://www.npmjs.com/package/config) module `config/local-development.json`, in 
> JSON over a [dotenv](https://www.npmjs.com/package/dotenv) module `.env` file.

The `config` package is great out of the box, but there is a couple of features it lacks holding it back, which we will
extend shortly.  
