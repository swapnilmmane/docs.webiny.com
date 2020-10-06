---
id: data-validation
title: Data Validation
sidebar_label: Data Validation
---

<iframe width="805" height="390" src="https://www.youtube.com/embed/sVCTIp9sng0" frameborder="0" allowfullscreen></iframe>

:::info Additional Resources
- [Code on CodeSandbox](https://codesandbox.io/s/04-data-validation-u2287) 
- [Code on GitHub](https://github.com/webiny/commodo-course/blob/master/04-data-validation.js)
:::

```js
import { withFields, string, number, boolean } from "@commodo/fields";

const Author = withFields({
  firstName: string(),
  lastName: string(),
  age: number(),
  isFamous: boolean()
})();

console.log(typeof Author);

const author = new Author();
console.log(author);

author.firstName = "John";
author.lastName = "Doe";
author.age = 25;
author.isFamous = false;
```

Upon assigning a value to a field of a model instance, the value is immediately validated on a data-type level. In other words, you cannot pass a string value to a number field, pass a boolean value to a string field, and so on.

So, if we were to change this value from number to string, an error would immediately be thrown, saying Invalid data type - number field “age” cannot accept value “25”. 

Note that, since this type of validation is always occurring upon assigning a value to a field, it is always triggered in a synchronous fashion.

Let’s undo this change so the error goes away.

With this data-type validation, we can also perform custom validation, which basically almost always reflects some part of your app’s business logic. We define it for each field separately, upon its definition, via the validation property, which represents a simple callback function in which we can perform any checks we might need, both synchronous and asynchronous.

For example, let’s try to ensure that the age field can only accept a value that is greater than or equal to 25. 

```js
age: number({
        validation: value => {
          if (value < 25) {
            throw new Error("Author must be at least 25 years old.");
          }
        }
      }),
```

Now that we have this in place, let’s try to assign number 24 to the age field.

```js
author.age = 24;
```

Notice that we didn’t get any errors in the console. This is because this type of validation needs to be triggered manually, by calling the validate method, which will trigger all validation callback functions, meaning all fields of a model instance will be validated. 

Let’s try to call the validate method on our author instance.

```js
await author.validate();
```

In the console, now we can clearly see an error with the Validation failed message has been thrown. The error object also contains a list of all invalid fields, with exact messages of the errors that might’ve been thrown inside of the field validation callback functions. 

Also, since callback functions can be both synchronous and asynchronous, note that we’ve called the validate method with the await keyword, otherwise our error would’ve never been properly caught.

To sum up, we’ve seen that we can perform two types of data validation on our model instances  - data-type validation and custom validation.

While the data type validation is immediately triggered upon assigning a value to a field and always in a synchronous fashion, custom validation must be triggered manually, by calling the asynchronous validate method. Remember to use the await keyword when calling it, otherwise the validation errors won’t be properly caught.
