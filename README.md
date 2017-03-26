# react-best-pratices
a small personal collection of best-practices for React that i either learned by myself
or read in some interesting articles.

feel free to open PRs if you know something that is missing!

## 1. components

### 1.1 use propTypes

I recommend using ``PropTypes`` in each of your components. It is a very useful part of documentation of your
component and also enables auto-completion in some IDEs. During development React will warn the developer
if there are mandatory PropTypes missing or when the developer passed down props with a wrong type. So PropTypes
is also some kind of static type checking directly integrated into React.

```js
class TodoListItem extends React.Component { 
  static propTypes = {
    description: React.PropTypes.string.isRequired,
    done: React.PropTypes.bool
  }
}
```
  
Additional Reading:
* [Official PropTypes documentation](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

### 1.2 avoid undefined checks of props with defaultProps

At some point you will encounter the following problem: You are writing a component which have optional props
to configure something. So if the developer uses your component and doesn't specify the optional prop you
will receive a undefined value. 

Most of the time it's more useful to assign a default-value to a prop and let the developer override
your prop if necessary. React already provides a great way to do this with ``defaultProps``! 

```js
class TodoListItem extends React.Component { 
  static propTypes = {
    description: React.PropTypes.string,
    done: React.PropTypes.bool
  }
  static defaultProps = {
    description: 'no description specified!',
    done: false
  }
}
```

Like ``propTypes`` you can define defaultProps as a static variable in your component. If you define your
``defaultProps`` in your component you don't always have to check for undefined or assign values in 
any lifecycle-hooks!

### 1.3 always call ``setState()`` with a function

This is a really important thing that many React developers don't know.
If you call ``setState()`` in your component with just a plain object this will work just fine, but it can produce some really hard to find errors!

This is why: ``setState()`` seems to be a synchronous action but actually it is asynchronous!
React batches state changes for performance reasons, so the state may not change immediately after ``setState()`` is called.
If you call setState() multiple times in one single update cycle it can be that your component only recognize one change because
of the batching!
That means you should not rely on the current state when calling ``setState()`` since you can’t be sure what that state will be!

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

Here’s the solution — pass a function to setState, with the previous state as an argument.

```js
this.setState(prevState => ({ done: !prevState.done }))
```

Additional reading:
* [Eric Elliott - Medium](https://medium.com/javascript-scene/setstate-gate-abc10a9b2d82#.uhq9u57pd)
* [Musefind - Medium](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8#.xndrx3uif), 
* [Tomas Carnecky - Medium](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3#.41mpihie2)

### 1.4 put async actions into componentDidMount

Sometimes your component has to fetch data. So where do you put asynchronous actions in your react component? The answer is: componentDidMount

There are actually "three" possible places:
1. componentWillMount
2. constructor
3. componentDidMount

the ES6 style constructor method is actually the same as componentWillMount so you can put your code there instead of
creating the componentWillMount lifecycle hook.

But the problem with componentWillMount is that the component actually will render at least one time before your data is fetched. This can create some misunderstanding because some developers might think they don't need to take care of the components default state because the data "should" already be there before the initial render.

But there are two reasons to put async actions into componentDidMount:
1. it makes clear that the component already mounted so you need to take care of the state of your component before the data is fetched
2. if you want to do SSR componentWillMount can be fired multiple times so it's a good idea to get used to put your code into cDM 

Additional reading:
* [David Ceddia - daveceddia.com](https://daveceddia.com/where-fetch-data-componentwillmount-vs-componentdidmount/?utm_content=buffer8ffb3&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
