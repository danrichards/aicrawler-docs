# Extending Heuristics


Heuristics is a bunch of static methods that return `boolean` values. We may
build creative tools that traverse a node structure and score nodes using
these static methods without memory getting out of hand.

Advice when extending Heuristics:

1. Heuristic method names should never intersect with argument names.
2. Heuristic method names should not have integers in them.
3. Usage of `arg`, `arr`, `boolean` and `text` helper methods is encouraged.
4. Heuristic methods should `return static::subset(...)` instead of true when possible.
5. Subset methods (e.g. `children()`) should not `return static::subset(...)`.
6. Heuristics methods should follow the following interface: `public static function heuristic(AiCrawler &$node, array $args = [])`.
7. Heuristics methods should be snake_case (sorry).