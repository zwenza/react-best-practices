# react-best-pratices
a small personal collection of best-practices for react i either learned by myself
or just read in some interesting articles.

feel free to open PRs if you know something that is missing!

## 1. components

### 1.1 use propTypes

I recommend using ``propTypes`` in each of your components. It is a very useful part of documentation of your
component and also enables auto-completion for some IDEs. In dev mode React will also warn the developer
if there are mandatory propTypes missing or the developer passed down props with a wrong time. So propTypes
is also some sort of static type checking directly integrated into React.

```js
export default class ExampleComponent extends Component { 
  static propTypes = {
    title: React.PropTypes.string.required,
    description: React.PropTypes.string
  }
}
```
  
more info about propTypes here: [propTypes documentation](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

### 1.2 avoid undefined checks of props with defaultProps

At some point you will encounter the following problem: You write a component which have optional props
to configure something. So if the developer uses your component and doesn't specify the optional prop you
will receive a undefined value. 

Most of the time it's much more useful to assign a default-value to a prop and let the developer override
your prop if necessary. And React already provides a great way to do this with ``defaultProps``! 

```js
export default class ExampleComponent extends Component { 
  static defaultProps = {
    description: 'no description given!'
  }
}
```

Like ``propTypes`` you can define defaultProps as a static variable on your component. If you define your
``defaultProps`` on your component you doesn't need to always check for undefined or assign values in 
any lifecycle-hooks!

### 1.3 always call ``setState()`` with a function

This is a really important thing that many React developers doesn't know.
If you call ``setState()`` in your component with just a plain object this will work fine but can
produce some really hard to find errors!

This is why: ``setState()`` seems to be a synchronous action but actually it is asynchronous!
React batches state changes for performance reasons, so the state may not change immediately after ``setState()`` is called.
If you call setState() multiple times in one single update cycle it can be that your component only recognize one change because
of the batching!
That means you should not rely on the current state when calling ``setState()`` since you can’t be sure what that state will be!

The problem can be fixed by calling ``setState()`` with a function which will receive the previousProps as an argument and
return the new state. This way react can queue your ``setState()`` calls and you can be sure that all calls will be executed
in the currect order!

Sometimes it's obvious that multiple ``setState()`` calls happens right after each other in the code

```js
this.setState({ ...this.state, test: true });
if (condition) {
    this.setState({ ...this.state, test: false });
}
```

But there are also some really tricky ones for example: combination of ``componentWillReceiveProps`` and an input ``onChange`` event handler.

So i would always recommend using ``setState()`` only with function! 
It's not hard to get used to it and you can always be sure that react executes all of
your ``setState()`` calls.

Here’s the solution — pass a function to setState, with the previous state as an argument.

```js
this.setState(prevState => ({ test: !prevState.test }))
```

Some resources about this:
[Musefind - Medium](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8#.xndrx3uif), 
[Tomas Carnecky - Medium](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3#.41mpihie2)
