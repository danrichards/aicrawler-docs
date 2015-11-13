# Heuristics

- [Arguments](#arguments)
- [Basic Usage](#basic)
- [Subset Usage](#subset)
- [Nested Usage](#nested)
- [Why all the statics?](#statics)
- [Helper Methods](helpers.md) $$^$$
- [Extending Heuristics](extending.md) $$^$$

>The objective of a heuristic is to produce a solution in a reasonable time frame that is good enough for solving the problem at hand. This solution may not be the best of all the actual solutions to this problem, or it may simply approximate the exact solution. But it is still valuable because finding it does not require a prohibitively long time. ~ wikipedia

All methods and properties in the `Heuristics` class are static. All `public` methods look at a node return a `boolean` value. 

You will also notice that each method has a similar interface. e.g.

```
public static function characters(AiCrawler &$node, array $args = [])
```

## Arguments <a name="arguments"></a>

Passing the node will allow our heuristics access to anything they might need. All arguments are passed as an array. This will later simplify storing the criteria for our heuristics in a configuration.

Another in-place convention is using a static object property for default arguments. e.g.

```
protected static $characters = [
    'characters' => true,
];
```

Missing arguments will fall back to the class property and there are [helper methods](helpers.md) so accessing arguments is readable and errors may be handled. There is also a property called `$defaults` for more general properties which acts a secondary fall back. If a heuristic requires an argument it cannot find, an `InvalidArgumentException` will be thrown. You should keep this in mind when [extending the Heuristics class](Heuristics/extending.md).


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

$assertion = Heuristics::characters($node, $args)); // true / false
```
    
    
#### Most rules are designed accept an variety of args:

Do any words in the div's text intersect with the words provided.

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

    
Does the div's text (including the text of its descendants) have at least two matches from the first expression and at least one match from the second expression.
    
```
$args = [
    'words' => [
        '(cod(ing|ed|e)|program|language|php)/' => 2,
        '(a(rtificial\s)?i(ntelligence)?|machine\slearn)/' => 1
    ],
    'regex' => true,
    'descendants' => true,
];
```
    
## Subset Usage <a name="subset"></a>

The Symfony DOMCrawler has facilities like `children()`, `parents()`, `siblings()` which have corresponding rules in the `Heuristics` class. These are referred to as subset (or relation) methods.

```
// Provided a crawler with content...
$node = $crawler->filter('div[class="entry-content"] ul')->first();
```

Does the node have at least three children?
   
```    
$args = ['children' => 3];
```
    
Synonymous with the above.

```
$args = [
    'type' => 'children',
    'matches' => 3
];
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
```
    
The logic for how assertions are made with subset methods is contained in the `on` method. How [matching is evaluated](on.md#matching) with the [`on()`](on.md) methods can be quite expressive.

## Nested Usage <a name="nested"></a>


    
## Why all the statics?

The `Heuristics` class was made entirely static so it would be difficult to build subsequent abstractions which put memory at risk. The Symfony DOMCrawler provides tools for iterating the DOM which we can apply heuristic assertions and then apply scoring. A combination of rule configurations, how we score those rules, and how we iterate the DOM will determine the usefulness of our tools.

