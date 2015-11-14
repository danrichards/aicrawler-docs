#AiCrawler

Leverage Ai design patterns using heuristics with the Symfony DOMCrawler.


## Table of Contents

- [Reading the docs](#reading)
- [AiCrawler](AiCrawler/README.md)
   - [Scorable](AiCrawler/scorable.md)
   - [Extra](AiCrawler/extra.md)
   - [Heuristics](AiCrawler/Heuristics/README.md)
- [AiResponses](AiResponses/README.md)
- [AiScrapers](AiScrapers/README.md)
- [Release Notes](#notes)
- [Contributing](#contributing)
- [License](#license)


## Quickstart<a name="quickstart"></a>

The [AiCrawler](AiCrawler/README.md) package has the responsibility of making boolean assertions on a node in the HTML DOM. It comes with a straight-forward [data point trait](AiCrawler/scorable.md) which will record the results of your [heuristics (rules)](AiCrawler/Heuristics/README.md) for a given "item" or context.

## Install with Composer<a name="install"></a>

>$ composer require dan/aicrawler dev-master


## Version 0.0.1<a name="notes"></a>

- A `Heuristics` class with some cool rules to get you started.
- A `Scorable` trait is on our `AiCrawler`class so there is a pattern for data points.
- A `Extra` trait is on our `AiCrawler` class so there is a pattern for storing extra data.


## Todo<a name="todo"></a>

- [Search Github](https://github.com/danrichards/aicrawler/search?utf8=%E2%9C%93&q=todo)
- Finish related projects. See [AiResponders](https://github.com/danrichards/airesponders), [AiScrapers](https://github.com/danrichards/aiscrapers), and [Larascrape](https://github.com/danrichards/larascrape).


## Contributing<a name="contributing"></a>

1. [Fork](https://github.com/danrichards/aicrawler) this [project](https://github.com/danrichards/aicrawler) on GitHub.
2. Existing [unit tests](https://github.com/danrichards/aicrawler/tree/master/tests) must pass.
3. Contributions must be unit tested.
4. New heuristics should be portable (have few or no dependencies).
5. New heuristics should have helpful doc blocks.
6. Submit a pull request.


### Documentation<a name="documentation"></a>

- Follow [PSR-2](http://www.php-fig.org/psr/psr-2/).
- Add PHPDoc blocks for all classes, methods, and functions
- Omit the `@return` tag if the method does not return anything
- Add a blank line before `@param`, `@return` or `@throws`

Any issues, please [report here](https://github.com/danrichards/aicrawler/issues)


## License<a name="license"></a>

AiCrawler is free software distributed under the terms of the [MIT license](http://opensource.org/licenses/MIT).