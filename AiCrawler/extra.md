# Extra

`Extra` is a trait available on the [`AiCrawler`](README.md) class which essentially manages a key / value store on each node. If you absolutely must store data on a node in some abstraction, `$extra` is the place to do it.

If a problem can be solve eloquently by avoiding storing data in the `$extra` property, I recommend that option. However, there are circumstances where persisting something to the node will come in handy so a pattern is place for such circumstances.

This trait comes with two versatile methods `setExtra()` and `getExtra()` which allow to you interact with the key / value store in a couple different ways. If you think you might need to store info a node, take a minute and [review the code on github](https://github.com/danrichards/core/blob/master/src/Dan/Core/Support/Traits/Extra.php).