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