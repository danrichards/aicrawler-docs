# AiCrawler

Leverage Ai design patterns using heuristics with the Symfony DOMCrawler.

## What is it?

The [Symfony DOMCrawler](http://symfony.com/doc/current/components/dom_crawler.html) does great job building special-purpose crawlers. AiCrawler provides the means to build intelligent all-purpose scrapers, data-analysis tools or something specialized. The over-arching goal for this extension is to take a AI approach towards [scoring nodes on the DOM](scorable.md), based on interesting characteristics that can be defined by a heuristic. 

Scoring nodes using heuristics allows us to use our creativity to do some of the following things: 

1. Learn / gather something special in a large arrangement of data.
2. Reduce dependency on the DOM for finding our tool's payload.

## Why all the statics?

The `Heuristics` class was made entirely static so it would be difficult to build subsequent abstractions which put memory at risk. All `public` methods look at a node return a `boolean` value. The Symfony DOMCrawler provides tools for iterating the DOM which we can combine heuristic assertions and apply scoring . A combination of rule configurations, how we score those rules, and how we iterate the DOM will determine how useful our tools become.

You will also notice that each method has a similar interface. e.g.

    public static function characters(AiCrawler &$node, array $args = [])

Passing the node will allow our heuristics access to anything they might need. All arguments are passed as an array. This will later simplify storing the criteria for our heuristics in a configuration.

Another in-place convention is using a static object property for argument defaults. e.g.

    protected static $characters = [
        'characters' => true,
    ];

Any missing arguments will fall back to the class property. There is also a property called `$defaults` for more general properties which acts a secondary fall back. If a heuristic requires an argument it cannot find, an `InvalidArgumentException` will be thrown. You should keep this in mind when [extending the Heuristics class](Heuristics/extending.md).