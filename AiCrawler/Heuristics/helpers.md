# Helper Functions

## arg(array $args, $key, $function = null)

Get the key or return the default.

    @param $args
    @param $key
    @param null $function
    
    @return mixed
    @throws InvalidArgumentException

## arr(array $args, $key, $explodeDelimiter = false)

Get an `array` argument. Optionally explode a string. Using `$explodeDelimiter = ''`, empty string will explode a string into it's characters.

    @param $args
    @param $key
    @param bool $explodeDelimiter
    
    @return bool
    @throws InvalidArgumentException
     

## boolean(array $args, $key)

Sugar so the dev knows the arg should be `boolean`.

    @param $args
    @param $key
    
    @return bool
    @throws InvalidArgumentException

## text(AiCrawler &$node, $position = 0, $descendants = false, $case_sensitive = false, $ascii = false)

Get the text for a node.

    @param AiCrawler $node
    @param int $position
    @param bool $descendants
    @param bool $case_sensitive
    @param bool $ascii
    
    @return string

## subset(AiCrawler &$node, $args = [], $condition = true, $function = null)

When a rule hits, array_intersect with `static::$subsetFunctions`
and run the additional rules provided. This enables us to nest
our rules and continue down some rabbit hole to make an assertion.

Nesting rules is like applying the AND operator. Neat!

    @param AiCrawler $node
    @param array $args
    @param bool $condition
    @param null $callingFunction
    
    @return bool