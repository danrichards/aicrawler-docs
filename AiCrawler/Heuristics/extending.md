# Extending Heuristics


The Heuristics class is a collection of static methods that return `boolean` values. Think of the class as a set of rules in which we may score (see [Scorable](../scorable.md)) nodes (make assertions) on the DOM. All the arguments are provided in one array `$args`, so we may easily store configurations and use our heuristic functions dynamically. This pattern (see [AiScrapers](../../AiScrapers/README.md) for example) in combination with the what you already get with the excellent [Symfony DOMCrawler](http://symfony.com/doc/current/components/dom_crawler.html) allow us to build abstractions that iterate the DOM and hit nodes with heuristics (rules) to determine some interesting.

## Extending Heuristics

### Best Practices

1. Heuristics methods should follow the following interface: 

   `public static function my_heuristic(AiCrawler &$node, array $args = [])`
2. Heuristic method names should never intersect with argument names.
3. Heuristic method names should not have integers in them.
4. Usage of static class properties that match method names is encouraged.
5. Usage of `arg`, `arr`, `boolean` and `text` helper methods is encouraged.
6. Heuristic methods should `return static::subset(...)` instead of true when possible.
7. Subset methods (e.g. `children()`) should not `return static::subset(...)`.

### Example Custom Heuristics Class
    
    class CustomHeuristics extends Heuristics
    {
    
        /**
         * Even function. Determine if a node is even among its siblings.
         *
         * @param AiCrawler $node
         * @param array $args
         *
         * @return bool
         */
        public static function even(AiCrawler &$node, array $args = [])
        {
            try {
                return $node->previousAll()->count() % 2 != 0;
            } catch (InvalidArgumentException $e) {
                return false;
            }
        }
    
        /**
         * Odd function. Determine if a node is odd among its siblings.
         *
         * @param AiCrawler $node
         * @param array $args
         *
         * @return bool
         */
        public static function odd(AiCrawler &$node, array $args = [])
        {
            try {
                return $node->previousAll()->count() % 2 == 0;
            } catch (InvalidArgumentException $e) {
                return true;
            }
        }
    }