---
id: introduction
title: Introduction
sidebar_label: Introduction
---

<iframe width="805" height="390" src="https://www.youtube.com/embed/i9xRDdqCzjk" frameborder="0" allowfullscreen></iframe>

:::info Additional Resources
- [Commodo GitHub repository](https://github.com/webiny/commodo)
:::

[Commodo](https://github.com/webiny/commodo) is a library of higher order functions that let you define and compose rich data models.

In the following example, we might define an `Animal` model, which consists of a couple of fields - `name`, `age`, `isAwesome`, and `about`.

```js
import { withFields, string, number, boolean } from "@commodo/fields";

const Animal = withFields({
    name: string({
        validation: value => {
            if (!value) {
                throw Error("A pet must have a name!");
            }
        }
    }),
    age: number(),
    isAwesome: boolean(),
    about: fields({
        value: {},
        instanceOf: withFields({
            type: string({ value: "cat" }),
            dangerous: boolean({ value: true })
        })()
    })
})();
```

Once we have a model defined, we can then instantiate it:

```js
const animal = new Animal();
```

...populate it with some data: 

```js
animal.populate({ age: 7 });
await animal.validate(); // Throws a validation error - name must be defined.
```

...and finally, validate it:

```js
animal.name = "Garfield";
await animal.validate(); // All good.
```

All of this is made possible with the `withFields` higher order function, and a couple of built in fields like `string`, `number`, and `boolean`, all imported from the [`@commodo/fields`](https://github.com/webiny/commodo/tree/next/packages/fields) package.

There are also other packages and higher order functions that we can use to add more functionality to our models, like [`@commodo/name`](https://github.com/webiny/commodo/tree/next/packages/fields), [`@commodo/hooks`](https://github.com/webiny/commodo/tree/next/packages/hooks), and [`@commodo/fields-storage`](https://github.com/webiny/commodo/tree/next/packages/fields-storage), which you can, respectively, use to assign a unique name to your models, register hooks and trigger pieces of code on certain events, and even connect your models to a real database, so your data becomes persistent.

All of this, and more, will be covered in the following sections, so feel free to [continue](/docs/api-development/commodo/core-ideas) reading. 
