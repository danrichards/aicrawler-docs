# Scoreable

`Scoreable` is a trait available on the `AiCrawler` class which essentially manages an associative array of associative arrays.


Example of what the `scores` property might look like on your AiCrawler instance:
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

>Important: The `Heuristics` class will never augment the scores of a node. The only interaction with the Scoreable trait it has is the [`after_hitting()`](Heuristics/after_hitting.md) and [`after_missing()`](Heuristics/after_missing.md) methods.


