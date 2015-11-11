# Helper Functions

## arg()

Get the key or return the default.

    @param $args
    @param $key
    @param null $function
    
    @return mixed
    @throws InvalidArgumentException

## arr()

Get an array argument.

    @param $args
    @param $key
    @param bool $explodeDelimiter
    
    @return bool
    @throws InvalidArgumentException
     

## boolean()

Sugar so the dev knows the arg should be boolean.

    @param $args
    @param $key
    
    @return bool
    @throws InvalidArgumentException

## text()

Get the text for a node.

    @param AiCrawler $node
    @param int $position
    @param bool $remove_children
    @param bool $case_sensitive
    @param bool $ascii
    
    @return string

## subset()

On the condition passing, array_intersect with static::$subsetFunctions
and run all those methods on the node provided. This enables us to nest
our rules and continue down some rabbit hole to make an assertion.

Nesting rules is like applying the AND operator. Neat!

    @param AiCrawler $node
    @param array $args
    @param bool $condition
    @param null $callingFunction
    
    @return bool