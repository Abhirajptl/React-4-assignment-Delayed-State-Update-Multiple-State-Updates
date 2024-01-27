Q.1 - explain why the console.log shows the older value of count
answer - In React, state updates are asynchronous, which means that when you call setCount(count + 1), React doesn't immediately update the state. Instead, it schedules an update and continues with the rest of the function. Therefore, when you log the count immediately after calling setCount, you are still logging the older value of count because the state update hasn't taken effect yet.

To see the updated value of count, you can use the useEffect hook to log the value after the state has been updated. Here's how you can modify your code:

function App() {
  const [count, setCount] = React.useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  React.useEffect(() => {
    console.log(count); // Now you will see the updated value of count in console
  }, [count]);

  return (
    <div>
      <p>Button is clicked {count} times</p>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

By adding the useEffect hook with the dependency array [count], you ensure that the logging occurs after the state has been updated, and you will see the updated value of count in the console.



Q.2 - explain why count value is not updated to 3 as expected
answer - In React, the useState hook is asynchronous, meaning that the state update is not immediately reflected in the variable that holds the state value. When you call setCount, React schedules an update to the state, but it doesn't happen instantly.

In your handleClick function, you are calling setCount three times in quick succession:
Since these state updates are asynchronous, the value of count doesn't get immediately updated after each call to setCount. Therefore, when you log the value of count immediately after the three setCount calls, you're still logging the old value of count.

To see the updated value of count, you should use the useEffect hook to log the value after the component has been re-rendered due to the state update:
Now, the useEffect hook will log the updated value of count after each re-render triggered by the state updates.

Now Propose an approach to achieve the expected behavior.

import React from 'react'

function App() {
  const [count, setCount] = React.useState(0);

  const handleClick = () => {
    setCount(setCount(prevCount => prevCount + 1));
    setCount(setCount(prevCount => prevCount + 1));
    setCount(setCount(prevCount => prevCount + 1));
		console.log(count);
  };

  return (
    <div>
      <p>Button is clicked {count} times</p>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App

Here we should use the functional form of the updater function:

setCount(prevCount => prevCount + 1);

This ensures that you're working with the most recent state value.

- By using `prevCount => prevCount + 1`, each call to `setCount` uses the most recent state value.
- It ensures that each update is based on the state from the previous update.
- After clicking the button, `count` correctly increments by 3.