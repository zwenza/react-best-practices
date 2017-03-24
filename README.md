# react-best-pratices
a small personal collection of best-practices for react i found during my learning process.
I will update this every time i find something new to make better 

## 1. components

### 1.1 use propTypes

I recommend using prop-types in each of your components. It is a very useful part of documentation of your
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
  
more info about propTypes here: https://facebook.github.io/react/docs/typechecking-with-proptypes.html

### 1.2 avoid undefined checks of props with defaultProps

At some point you will encounter the following problem: You write a component which have optional props
to configure something. So if the developer uses your component and doesn't specify the optional prop you
will receive a undefined value. 

Most of the time it's much more useful to assign a default-value to a prop and let the developer override
your prop if necessary. And React already provides a great way to do this! 

```js
export default class ExampleComponent extends Component { 
  static defaultProps = {
    description: 'no description given!'
  }
}
```

Like propTypes you can define defaultProps as a static variable on your component. If you define your
defaultProps on your component you doesn't need to always check for undefined or assign values in 
any lifecycle-hooks!
