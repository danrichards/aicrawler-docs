# Extending Heuristics

The `Heuristics` class is a collection of static methods that return `boolean` values. When extending `Heuristics`, you should make your best effort to use the patterns it has put in place to work for you. If you don't it might work against you. :grin:

## Best Practices

1. Heuristics methods should follow the following interface: 

   `public static function awesome_heuristic(AiCrawler &$node, array $args = [])`
2. Heuristic method names should never intersect with argument names of other heuristics.
3. Heuristic method names should not have integers in them, subset methods use these!
4. Usage of static class properties that match method names is encouraged.
5. Usage of `arg`, `arr`, `boolean` and `text` helper methods is encouraged.
6. Heuristic methods should `return static::subset(...)` instead of true when possible.
7. Subset methods (e.g. `children()`) should not `return static::subset(...)`.
8. Heuristic methods should not persist data to the node's `$extra` property when at all possible.

## Example Custom Heuristics Class
    
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