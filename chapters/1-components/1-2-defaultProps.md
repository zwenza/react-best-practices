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
