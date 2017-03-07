+++
title = "Stardog 1.0"
date = "2012-06-18"
author = "Kendall Clark"
series = ["Release Announcement"]
discourseUsername = "kendall"
type = "post"
+++

Today I'm happy to announce the release of [Stardog
1.0](http://stardog.com/), the fastest, smartest, and easiest to use RDF
database on the planet. Stardog fills a hole in the Semantic Technology
(and NoSQL database) market for an RDF database that is fast, zero config,
lightweight, and feature-rich.<!--more-->

## Speed Kills
 
RDF and OWL are excellent technologies for building data integration and
analysis apps. Those apps invariably require complex query processing,
i.e., queries where there are lots of joins, complex logical conditions to
evaluate, etc. Stardog is targeted at query performance for *complex SPARQL
queries*. We publish [performance data](http://stardog.com/docs/performance)
so you can see how we're doing.

## Braindead Simple Deployment
 
Winners ship. Period.
 
We care very much about simple deployments. Stardog works out-of-the-box
with minimal (none, typically) configuration. You shouldn't have to fight an
RDF database for days to install or tune it for great performance. Because
Stardog is pure Java, it will run anywhere. It *just works* and it's *damn
fast*. You shouldn't need to buy and configure a cluster of machines to get
blazing fast performance from an RDF database. And now you don't have to.

## One More Thing...OWL Reasoning
 
Finally, Stardog has the deepest, most comprehensive, and best OWL reasoning
support of any commerical RDF database available.

Stardog 1.0 supports RDFS, OWL 2 QL, EL, and RL, as well as OWL 2 DL
schema-reasoning. It's also the only RDF database to support closed-world
integrity constraint validation and automatic explanations of integrity constraint
violations. 

If you care about data quality, Stardog 1.0 is worth a hard look.

## Free Evaluation

Stardog 1.0 is available for a free 30-day evaluation; [download it
now](http://stardog.com/dl).

When you're ready to talk about licensing terms and support contracts, drop
us an [email](mailto:sales@clarkparsia.com).
