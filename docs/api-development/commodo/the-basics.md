---
id: the-basics
title: The Basics
sidebar_label: The Basics
---

<iframe width="805" height="390" src="https://www.youtube.com/embed/NbSmE2EFP3Y" frameborder="0" allowfullscreen></iframe>

:::info Additional Resources
- [Code on CodeSandbox](https://codesandbox.io/s/03-the-basics-465xl) 
- [Code on GitHub](https://github.com/webiny/commodo-course/blob/master/03-the-basics.js)
:::

### Creating a simple model

It’s time to create our first model. To get started, let’s import a couple of things from the `@commodo/fields` package:

```js
import { withFields, string, number, boolean } from "@commodo/fields"; 
```

The model we’re about to create is a simple `Author` model, which is going to look something like this:

```js
const Author = withFields({
  firstName: string(),
  lastName: string(),
  age: number(),
  isFamous: boolean()
})();
```

As we can see, we are utilizing the `withFields` function that accepts an objec as its first and only argument, which is a list of all fields we want our data model to have. 

We have `firstName` and `lastName` as simple `string` fields, `age` as a `number`, and `isFamous` as a `boolean` field.

Before moving on, let’s cover a couple of things here.

### Calling the `withFields` higher order function

First, notice that we are making a function call in the last line of code. 

```js
const Author = withFields({
  firstName: string(),
  lastName: string(),
  age: number(),
  isFamous: boolean()
})(); // <- Notice the function call here.
```

This exists because the `withFields` is a [higher order function](https://en.wikipedia.org/wiki/Higher-order_function), meaning, instead of immediately returning a model with fields, it’s actually returning a new function, which we can call with either no arguments, or we can pass an existing model, to which the fields will be appended. 

In our case, we did not pass an existing model, which basically means we’re about to create a brand new one. 

:::tip
Not only this approach allows us to append the fields to an existing or a new model, but also to create multiple field collections and then compose them in order to define different models in a flexible way. More on this in can be found in the [Custom HOFs and Function Composition](/docs/api-development/commodo/custom-hofs-and-function-composition) article. 
:::

### What exactly is a model?

If we tried to execute a simple `console.log(typeof Author);` like this, we’d see a model is just a simple function. 

```js
console.log(typeof Author); // Returns "function".
```

And if we would instantiate it with the keyword `new`, we’d get a simple object, that consists of a couple of properties. Most notably, the actual fields we’ve defined but also some functions that will help us along the way - like `getFields`, `getField`, `populate`, and so on.

```js
const author = new Author();
console.log(author);

// The above console.log will return the following:
baseFn {__withFields: Object, getFields: function getFields(), getField: function getField(), populate: function populate(), validate: function validate()…}
__withFields: Object
getFields: function getFields() {}
getField: function getField() {}
populate: function populate() {}
validate: function validate()
clean: function clean() {}
isDirty: function isDirty() {}
<constructor>: "baseFn"
```

For now, no need to worry about the internals too much. Just have in mind, that when we talk about models, we’re actually talking about plain old functions, which you’re already using on a daily basis.

### Assigning data
Now that we've seen how we can create an instance of a model, with the keyword `new`, let’s try assigning some data to it.

```js
author.firstName = "John";
author.lastName = "Doe";
author.age = 35;
author.isFamous = false;
```

Assigning data can be achieved this way and it will work, but, to make it a bit easier, we can also use the `populate` method that the `withFields` function provides for us: 

So, let’s create another instance of the Author model and see how that looks:

```js
const author2 = new Author();
author2.populate({
  firstName: "Jane",
  lastName: "Doe",
  age: 30,
  isFamous: true
});
```

Remember - both approaches produce the same result. The approach we'll use depends on the particular situation we're in.

:::tip
As with regular function instantiation, multiple model instances that we create are completely independent. Meaning, changing a field on one instance won’t affect the other, and vice versa.
:::
