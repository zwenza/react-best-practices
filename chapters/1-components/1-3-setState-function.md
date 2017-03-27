### 1.3 always call ``setState()`` with a function

This is a really important thing that many React developers don't know.
If you call ``setState()`` in your component with just a plain object this will work just fine, but it can produce some really hard to find errors!

This is why: ``setState()`` seems to be a synchronous action but actually it is asynchronous!
React batches state changes for performance reasons, so the state may not change immediately after ``setState()`` is called.
If you call setState() multiple times in one single update cycle it can be that your component only recognize one change because
of the batching!
That means you should not rely on the current state when calling ``setState()``?since you can’t be sure what that state will be!

The problem can be fixed by calling ``setState()`` with a function, which will receive the previousProps as an argument and
return the new state. This way react can queue your ``setState()`` calls and you can be sure that all calls will be executed
in the correct order!

Sometimes it's obvious that multiple ``setState()`` calls happen right after each other in the code

```js
this.setState({ ...this.state, done: true });
if (condition) {
    this.setState({ ...this.state, done: false });
}
```

But there are also some really tricky ones for example: combination of ``componentWillReceiveProps`` and an input ``onChange`` event handler.

So i would always recommend using ``setState()`` only with a function as an argument! 
It's not hard to get used to it and you can always be sure that react executes all of
your ``setState()`` calls.

Here’s the solution?—?pass a function to setState, with the previous state as an argument.

```js
this.setState(prevState => ({ done: !prevState.done }))
```

Additional reading:
* [Eric Elliott - Medium](https://medium.com/javascript-scene/setstate-gate-abc10a9b2d82#.uhq9u57pd)
* [Musefind - Medium](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8#.xndrx3uif), 
* [Tomas Carnecky - Medium](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3#.41mpihie2)
