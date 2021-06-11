# State, Immutable Patters, Review

## Review - Make a Component

* Steps for creating a component

1. Create a file in the `components` directory
2. Import React from `'react'`
3. Make a function, preferably the same name as file
4. Export default the function


`CommentList.js`
```js
// Create a file in the `components` directory

// Import React from `'react'`
import React from 'react';

// Make a function, preferably the same name as file
const CommentList = () => {
  return (
    <h1>Hi </h1>
  );
};

// Export default the function
export default CommentList;
```

* Then we `import` the `CommentList` component in our main file (`App.js`)

---

## Review - Storybook

* Then we can *mock* the components in Storybook
  * Storybook helps us render and test the components in isolation

`comments.stories.js`
```js
storiesOf('CommentList')
  .add('A default commentList with some items', () => <CommentList />)
```

* And then we can use the attributes to pass into CommentList component

```js
storiesOf('CommentList')
  .add('A default commentList with some items', () => <CommentList number={12} array={[1,2,3,4]}/>)
```

* These are all passed to CommentList as `props` and can be accessed as `props.number` and `props.array`
  * This can any number of props to be used

