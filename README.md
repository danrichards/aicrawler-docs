#AiCrawler

Leverage Ai design patterns by using heuristics with the [Symfony DOMCrawler](http://symfony.com/doc/current/components/dom_crawler.html).


## Table of Contents

- [AiCrawler](AiCrawler/README.md)
   - [Scorable](AiCrawler/scorable.md)
   - [Extra](AiCrawler/extra.md)
   - [Heuristics](AiCrawler/Heuristics/README.md)
- [AiResponses](AiResponses/README.md)
- [AiScrapers](AiScrapers/README.md)
- [GitHub](https://github.com/danrichards/aicrawler)


## Quickstart<a name="quickstart"></a>

The [AiCrawler](AiCrawler/README.md) package has the responsibility of making boolean assertions on a node in the HTML DOM. It comes with a straight-forward [data point trait](AiCrawler/scorable.md) which will record the results of your [heuristics (rules)](AiCrawler/Heuristics/README.md) for a given "item" or context.

### Install with Composer<a name="install"></a>

```
$ composer require dan/aicrawler dev-master
```

### Trivial example<a name="install"></a>

```
$crawler = new AiCrawler('<html>...</html>');

$node = $crawler->filter('div[id="content-start"]');
$args = ['words' => 15];

// Does the content have at least 15 words?
$assertion = Heuristics::words($node, $args); // true / false
```

### A more expressive example

```
$crawler = new AiCrawler("<html>...</html>");

$args = [
    'elements' => [
        "elements" => "/p/ /blockquote/ /(u|o)l/ /h[1-6]/",
        "regex" => true,
        'words' => [
            'words' => 15,
            'descendants' => true,
            'words2' => [
                'words' => "/(cod(ing|ed|e)|program|language|php)/",
                'regex' => true,
                'descendants' => true
            ]
        ]
    ],
    'matches' => 3
]


/**
 * Do at least 3 of this div's children which are p, blockquote, ul, ol or any
 * h element AND contain at least 15 words (including text from the child's 
 * descendants) AND words such as coding, coded, code, program, language, php 
 * (including text from the child's descendants).
 */
$crawler->filter("div")->each(function(&$node) use ($args) {
    if (Heuristics::children($node, $args) {
        $node->setDataPoint("example", "words", 1);
    }
});
```
Sound interested? [Continue reading](AiCrawler/README.md), review the [`Heuristics`](AiCrawler/Heuristics/README.md) class or go right to a [similar example](AiCrawler/Heuristics/README.md#nested) with complete notes.