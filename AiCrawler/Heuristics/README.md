# Heuristics

>The objective of a heuristic is to produce a solution in a reasonable time frame that is good enough for solving the problem at hand. This solution may not be the best of all the actual solutions to this problem, or it may simply approximate the exact solution. But it is still valuable because finding it does not require a prohibitively long time. ~ wikipedia

## Give me an example

The `Heuristics` class is really meant to be used by other classes. But here are some trival usages:

Assume `$crawler` has a instance of `AiCrawler` with content loaded.
    
    $node = $crawler->filter('div[class="entry-content"] p')->first();
    
    // Nothing new here, we have a node to play with now.
        

### Tell me something about a node

Does the node have at least 15 words?
    
    $args = ['words' => 15];
    
    $assertion = Heuristics::characters($node, $args)); // true / false
    
Most rules are designed accept an variety of args:

    // Do any words in the node's text intersect with the words provided.
    
    $args = [
        'words' => "ai code coding",
        'matches' => "any"
    ];
    
    
    // Synonymous with the above, matches = "any" is default.
    
    $args = [
        'words' => ["ai", "code", "coding"]
    ];
    
    // Does the node's text (including the text of its descendant nodes) have at least
    // two matches from the first expression and at least one match from the second
    // expression.
    
    $args = [
        'words' => [
            '(cod(ing|ed|e)|program|language|php)/' => 2,
            '(a(rtificial\s)?i(ntelligence)?|machine\slearn)/' => 1
        ],
        'regex' => true,
        'descendants' => true,
    ];
    
### Tell me something about the relationships of a node

We also have the rules like `children`, `parents`, `siblings`. All these methods actually call the `on` method which just maps

    // Does the node have at least three children?
    
    $args = ['children' => 3];
    
    
    
    
    Heuristics may produce results by themselves, or they may be used in conjunction with optimization algorithms to improve their efficiency (e.g., they may be used to generate good seed values).