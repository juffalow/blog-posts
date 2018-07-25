---
title: 'React and setState method'
media_order: react-logo.png
taxonomy:
    tag:
        - JavaScript
        - React
header_image_file: react-logo.png
---

The first thing you read about `setState` - it is **asynchronous**. I understand it but still did not realize a couple of things.

===

## The problem

Of course I know that if you set some state variable and try to read it immediately, it won't be changed :

```javascript
// this.state.value is 0
this.setState({value: 1});
// this.state.value is till 0
```

But tricky question would be, what it does if you increment that state 3 times?

```javascript
// this.state.value is 0
this.setState({value: this.state.value + 1});
this.setState({value: this.state.value + 1});
this.setState({value: this.state.value + 1});
```

I asked it a couple of colleagues and maybe some of them did now ( or lucky guess ) the right answer, but didn't know why, or didn't know it. And I would say, this is not that tricky. Because `this.state.value` equals 0 you actually set it to 1 each time.

```javascript
this.setState({value: 0 + 1});
this.setState({value: 0 + 1});
this.setState({value: 0 + 1});
```

Or maybe, you have another case. What if you count the result value from some props? Let's say, we have some parent class:

```javascript
// App.js

import React, { Component } from 'react';
import IncrementButton from './IncrementButton';

class App extends Component {
  state = {
    value: 0,
  };

  onIncrementButtonClick = () => {
    this.setState({value: this.state.value + 1});
  }

  render() {
    return (
      <IncrementButton onClick={this.onIncrementButtonClick} value={this.state.value} />
    );
  }
}

export default App;
```

And `IncrementButton` looks like this:

```javascript
// IncrementButton.js

import React, { Component } from 'react';

class IncrementButton extends Component {
  state = {
    value: 0,
  };

  onButtonClick() {
    this.props.onClick();
    this.setState({value: this.props.value + 1});
  }

  render() {
    return (
      <button onClick={this.onButtonClick.bind(this)}>{this.state.value}</button>
    );
  }
}

export default IncrementButton;
```

If you click on the button, you call the parent method `onIncrementButtonClick` which will update the parent's state, i. e. some time later, `value` will be 1. Then you update the state of the `IncrementButton`. You take the value from the props and increment it. The problem is, that in the props, you have still the old value, therefore you update the state to '0 + 1`. The button after first click has value 1, but in fact, it should be 2.

## The solution

The solution for cases like this - when you calc / derive the next state from the actual state or from the props - is simple: use `setState` but pass a **function**:

```javascript
this.setState((prevState, props) => ({
  // ...
}));
```

Now, let's try it with the first example:

```javascript
// this.state.value = 0
this.setState((prevState) => ({value: prevState.value + 1}));
this.setState((prevState) => ({value: prevState.value + 1}));
this.setState((prevState) => ({value: prevState.value + 1}));
// will be 3 after update
```

The same applies on the second example. Modify the `onButtonClick` method of the `IncrementButton`:

```javascript
onButtonClick() {
  this.props.onClick();
  this.setState((prevState, prevProps) => { return {value: prevProps.value + 1} });
}
```

And now it works correctly! *Of course you should update also `onIncrementButtonClick` in the `App` class.*