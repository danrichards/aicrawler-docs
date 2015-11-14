# AiCrawler

Leverage Ai design patterns by using heuristics with the Symfony DOMCrawler.

## What is it?

The [Symfony DOMCrawler](http://symfony.com/doc/current/components/dom_crawler.html) does great job building special-purpose crawlers. AiCrawler provides the means to build intelligent all-purpose scrapers, data-analysis tools or something specialized. The over-arching goal for this extension is to take a AI approach towards [scoring nodes on the DOM](scorable.md), based on interesting characteristics that can be defined by a [heuristic](Heuristics/README.md). 

Scoring nodes using heuristics allows us to use our creativity to do some of the following things: 

1. Learn / gather something special in a large arrangement of data.
2. Reduce dependency on the DOM for finding our tool's payload.
 

## Reading the docs

Although I hate to be a jargonista, explaining things in documentation will be easier if you understanding the following:

* The words "heuristics" and "rules" are used synonymously.
* A rule "hitting" means the rule has asserted as true.
* A rule "missing" means the rule has asserted as false.