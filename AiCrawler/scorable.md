# Scoreable

`Scoreable` is a trait available on the [`AiCrawler`](README.md) class which essentially manages an associative array of associative arrays. The purpose of this trait is to provide a pattern for storing data points for a given item (or context) in which we have made assertions.

You might make a tool using the [`AiCrawler`](https://github.com/danrichards/aicrawler) package and the `scores` property on the current node (`AiCrawler` instance) might look as follows:
```
$this->scores = [
    'item' => [
        'datapoint' => 1,
        'datapoint2' => 4.5
    ],
    'headline' => [
        'elements' => 1.5
        'words' => 1,
    ],
    'content' => [
        'children' => 1,
        'arbitrary_alias' => 2
    ]
];
```

>Important: The [`Heuristics`](Heuristics/README.md) class will never augment the scores of a node. Currently, the only interaction with the `Scoreable` trait it has are the [`after_hitting()`](Heuristics/after_hitting.md) and [`after_missing()`](Heuristics/after_missing.md) methods.

## Why keep score?

That is for you to decide, what are you building? What problem are you trying to solve? If it's data extraction, the [`AiResponses`](../AiResponses/README.md) package is an example of how you can use these data points to conveniently extract information from the DOM.

It's also worth noting that we're **not** storing the result of the assertions in the `scores` property. Furthermore, the numbers are arbitrary and only mean anything to the tool you're building and the problem you're trying to solve.

By using the [`Scoreable`](https://github.com/danrichards/aicrawler/blob/master/src/Dan/AiCrawler/Scoreable.php) trait, you're subscribing to a convention that will help make all your tools look clear and uniform.

## Helper Methods

[`Scoreable`](https://github.com/danrichards/aicrawler/blob/master/src/Dan/AiCrawler/Scoreable.php) provides bunch of helper methods for easing storage of data points on your node. 