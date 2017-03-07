+++
title = "Stardog 1.2"
date = "2013-04-22"
author = "Kendall Clark" 
series = ["Release Announcement"]
discourseUsername = "kendall"
+++

Earlier today we released [**Stardog 1.2**](http://stardog.com/) which
you can download for evaluation right now.<!--more-->

Go on...I'll wait. Done? Awesome.

So what's new in Stardog 1.2? For the complete rundown, check out the
[changelog](http://stardog.com/docs/RELEASE_NOTES.txt). Yeah, we've
been busy...

## Performance

As always, Stardog's  performance is the most important feature. The 1.2 release is the fastest Stardog we've ever released. Performance improvements include:

* SPARQL query evaluation is faster, including a lower minimal overhead
* Durable transacted writes are faster
* Better caching of query plan rewritings improves reasoning perf
* Improved performance of write caching in transactions
* Applied better ordering to conjuncts makes reasoning query performance more deterministic
* Increased search indexing performance 

## Query Management

A new system for managing queries, which includes slow query logging, automatic query termination for queries that exceed a timeout, and query termination for queries by ID. This stuff makes deploying a public SPARQL endpoint backed by Stardog a lot easier, since long-running, rogue, or otherwise misbehaving queries can be managed effectively.

The slow query log is configurable---what counts as a slow query is interest-specific and user-relative. With slow query logging enabled, you can debug and then optimize slow queries.

All of this is available in native Stardog APIs including Java, HTTP, and from the CLI.

## CLI

The CLI is an important part of Stardog's great user experience. To make it even better, we've completely rewritten it for the 1.2 release. The CLI is now more consistent (examples, arguments, response codes, etc), is self-documenting, more rational, and easier to manage. We're also including Unix shell auto-completion support for the CLI.

We've also added comprehensive "man pages" to the [Stardog docs](http://stardog.com/docs/) for all CLI commands.

## Transactions

We also developed a new, modular, standalone library for Stardog's transactions; we call it `erg`. It uses asynchronous writes & memory-mapped logs to improve durable logging performance. Transaction failure recovery has been improved. If there is a fatal error in a transaction commit or rollback, the database will be taken offline and recovery of the failed transaction will be performed. Recovery is automatic, and the database will be brought back online once it is completed.  In the event recovery could not be completed without manual intervention, the database will remain offline as to minimize data loss.

## Security

The security layer is completely rewritten. This includes a systematic (internal) security audit, tons of new tests, and generally smaller overhead for security processing. The result? Better security that's faster and easier to manage. 

Other notable changes:

* `user edit` is replaced by `user addrole, removerole, disable, enable`
* `LIST` action is deprecated
* `REVOKE` and `EXECUTE` actions added
* Listing all users or roles requires different permissions
* Wildcards permitted when deleting permissions
* `ADMIN` resource for admin actions such as online & offline
* You must specify a password when creating a user
* Fixed vulnerability to billions laughs attacks

## Miscellaneous

Other improvements include:

* HTTP now supports SPARQL 1.1 Service Description
* Support for ARQ builtins `pi, e, sqrt, min, max`.
* Explanation system handles data inconsistencies
* Database, user, and role names now have a maximum size of 64
characters
* Updated to Sesame 2.7.0, Jena 2.10.0, Lucene 4.2.0, Shiro 1.2.1.

## Stardog 2.0

We're pushing on Stardog 2.0 already and expect it to be released
sooner than later. More details about that later on. Big items coming
in 2.0 include Stardog Web---a semantics-centric Javascript app
framework based on [Backbone.js](http://backbonejs.org/)---complete
SPARQL Update support, and the open source release of Stardog client
libraries for Java.
