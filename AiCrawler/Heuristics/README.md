# Heuristics

- [Arguments](#arguments)
- [Basic Usage](#basic)
- [Subset Usage](#subset)
- [Nested and Repeated Usage](#nested)
- [Why all the statics?](#statics)
- [Helper Methods](helpers.md)
- [Extending Heuristics](extending.md) 
- [Heuristics List](#heuristics)


>The objective of a heuristic is to produce a solution in a reasonable time frame that is good enough for solving the problem at hand. This solution may not be the best of all the actual solutions to this problem, or it may simply approximate the exact solution. But it is still valuable because finding it does not require a prohibitively long time. ~ wikipedia

All methods and properties in the `Heuristics` class are static. All `public` methods look at a node return a `boolean` value. 

You will also notice that each method has a similar interface. e.g.

```
public static function characters(AiCrawler &$node, array $args = [])
```

Passing the node will allow our heuristics access to anything they might need. All arguments are passed as an array. 

<a name="arguments"></a>
## Arguments 

Passing arguments as an array will later ease storing the criteria for our heuristics in a configuration.

Another in-place convention is using a static object property for default arguments. e.g.

```
protected static $characters = [
    'characters' => true,
];
```

Missing arguments will fall back to the class property and there are [helper methods](helpers.md) so accessing arguments is readable and errors may be handled. There is also a property called `$defaults` for more general properties which acts a secondary fall back. If a heuristic requires an argument it cannot find, an `InvalidArgumentException` will be thrown. You should keep this in mind when [extending the Heuristics class](Heuristics/extending.md).

### Matches

The most ubiquitous argument is `matches`. Although there are varying usages, here are the most common:

* all ~ all items in the given context match.
* **any (default)** ~ any (1 or more) of the items in the given context match.
* none ~ none of the items in the given context match.
* integer: 0 ~ synonymous with none.
* integer: n > 0 ~ at least n or more items in the given context match.

## Basic Usage <a name="basic"></a>

You will identify the real magic when you see `Heuristics` being used by later abstractions. But here are some trivial usages:

Assume `$crawler` has a instance of [`AiCrawler`](../README.md) with content loaded.

```    
// Provided a crawler with content...
$node = $crawler->filter('div[class="entry-content"]')->first();
```

### Tell me something about the `words` in the div

Does the div have at least 15 words?
    
```
$args = ['words' => 15];

$assertion = Heuristics::words($node, $args)); // true / false
```
    
    
#### Most rules are designed to accept a variety of args:

Do any words in the div's text intersect with the words provided?

```    
$args = [
    'words' => "ai code coding",
    'matches' => "any"
];

$assertion = Heuristics::words($node, $args)); // true / false
```
    
Synonymous with the above, `matches => "any"` is the default.
   
``` 
$args = [
    'words' => ["ai", "code", "coding"]
];

$assertion = Heuristics::words($node, $args)); // true / false
```

Does the div's text (including the text of its descendants) have at least two matches from the first expression and at least one match from the second expression?
    
```
$args = [
    'words' => [
        '/(cod(ing|ed|e)|program|language|php)/' => 2,
        '/(a(rtificial\s)?i(ntelligence)?|machine\slearn)/' => 1
    ],
    'regex' => true,
    'descendants' => true,
];

$assertion = Heuristics::words($node, $args)); // true / false
```
<a name="subset"></a>
## Subset Usage 

The Symfony DOMCrawler has facilities like `children()`, `parents()`, `siblings()` which have corresponding rules in the `Heuristics` class. These are referred to as subset (or relation) methods.

```
// Provided a crawler with content...
$node = $crawler->filter('div[class="entry-content"] ul')->first();
```

Does the node have at least three children?
   
```    
$args = ['children' => 3];

$assertion = Heuristics::children($node, $args)); // true / false
```
    
Synonymous with the above, using `on()` rule.

```
$args = [
    'type' => 'children',
    'matches' => 3
];

$assertion = Heuristics::on($node, $args)); // true / false
```
    
Any of the other rules on the `Heuristics` class can be an arg for `on()`

```
$args = [
    'on' => 'children',
    'elements' => [
        'elements' => '/li/',
        'regex' => true,
        'matches' => 'any',
    ],
    'words' => [
        'words' => 3,
        'descendants' => true
    ],
    'matches' => 'all'
];

$assertion = Heuristics::children($node, $args)); // true / false

```

### Subset evaluation with `on()` relation methods.

The `children()`, `parents()`, and `siblings()` methods expect args with subsequent rules, and our `matches` arg supports a multitude of new options. If you review the [matches logic](on.md#matches) for the [`on()`](on.md) rule you be on your way to building robust, expressive assertions.

<a name="nested"></a>
## Nested and Repeated Usage

Rules excluding the relation(`children()`, `parents()`, `siblings()`) rules call a special function called `subset()` before `true` is returned. This function looks for other rules included in the args and executes them until we assert `false`, or there are no more rules and finally we return `true`. 

Nested rules are resolved depth-first and we may use this to our advantage to write more efficient assertions.

For the following example, let's pretend we don't know what kind of node we have.

> Not knowing what node we have will prove useful later when iterating the DOM and making assertions. Instead of write proprietary code for special needs, we can throw a general rules at a bunch of nodes and see how things score. See [`AiScrapers`](../../AiScrapers/README.md).

Please use your intuition to guess what is happening here:

*Hint: Look at the assertion below the data structure before starting.*

```
[
    'elements' => "article div ul",
    'children' => [
        'elements' => [
            "elements" => "section p li"
            'words' => [
                'words' => 15,
                'descendants' => true,
                'words2' => [
                    'words' => "/(cod(ing|ed|e)|program|language|php)/",
                    'regex' => true,
                    'descendants' => true
                ]
            ]
        ],
        'matches' => 3
    ]
]

$assertion = Heuristics::elements($node, $args)); // true / false
```

I wholeheartedly hope your intuition did most of the work for you, let's summarize a true assertion:

1. `elements()` was called and we match either article, div or ul.
2. `elements()` calls `children()` and we're now examining sub-nodes.
3. `children()` calls `elements()` and we match section, p, or li.
4. `elements()` calls `words()` and we match 15 words.
5. `words()` calls `words()` **again** and match the regular expression.
6. Steps 3 ~ 5 repeat for each child of the article, div or ul element.
7. At least 3 or more children match the required criteria for `children()`
8. `elements()` returns true.

Nested rules are a simple way to apply **AND** logic to your assertions! 

<a name="statics"></a>
## Why all the statics?

The `Heuristics` class was made entirely static so it would be difficult to build subsequent abstractions which put memory at risk. The Symfony DOMCrawler provides tools for iterating the DOM which we can apply heuristic assertions and then apply scoring. A combination of rule configurations, how we score those rules, and how we iterate the DOM will determine the usefulness of our tools.

<a name="heuristics"></a>
## Heuristics List

* [characters()](AiCrawler/Heuristics/characters.md)
* [words()](AiCrawler/Heuristics/words.md)
* [punctuation()](AiCrawler/Heuristics/punctuation.md)
* [sentences()](AiCrawler/Heuristics/sentences.md)
* [questions()](AiCrawler/Heuristics/questions.md)
* [exclamatory()](AiCrawler/Heuristics/exclamatory.md)
* [p()](AiCrawler/Heuristics/p.md)
* [a()](AiCrawler/Heuristics/a.md)
* [img()](AiCrawler/Heuristics/img.md)
* [attributes()](AiCrawler/Heuristics/attributes.md)
* [attribute_values()](AiCrawler/Heuristics/attribute_values.md)
* [after_hitting()](AiCrawler/Heuristics/after_hitting.md)
* [after_missing()](AiCrawler/Heuristics/after_missing.md)
* [on()](AiCrawler/Heuristics/on.md)
* [children()](AiCrawler/Heuristics/children.md)
* [parents()](AiCrawler/Heuristics/parents.md)
* [siblings()](AiCrawler/Heuristics/siblings.md)

