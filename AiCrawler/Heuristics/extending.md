# Extending Heuristics


Heuristics is a bunch of static methods that return `boolean` values. We may
build creative tools ([see AiScrapers](../AiScrapers/README.md)) that traverse a node structure and score nodes using
these static methods without memory getting out of hand.

Advice when extending Heuristics:

1. Heuristics methods should follow the following interface: 

    `public static function my_heuristic(AiCrawler &$node, array $args = [])`
    
2. Heuristic method names should never intersect with argument names.
3. Heuristic method names should not have integers in them.
4. Usage of `arg`, `arr`, `boolean` and `text` helper methods is encouraged.
5. Heuristic methods should `return static::subset(...)` instead of true when possible.
6. Subset methods (e.g. `children()`) should not `return static::subset(...)`.

