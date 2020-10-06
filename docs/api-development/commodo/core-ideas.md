---
id: core-ideas
title: Core Ideas
sidebar_label: Core Ideas
---

<iframe width="805" height="390" src="https://www.youtube.com/embed/RpTWltA2d4I" frameborder="0" allowfullscreen></iframe>

:::info Additional Resources
- [Commodo GitHub repository](https://github.com/webiny/commodo)
:::

### Is Commodo just another ORM tool?
In the [introduction](/docs/api-development/commodo/introduction) section, we mentioned that you can connect your models to a real database in order to persist your data. So, one of the first questions you might ask yourself is, is Commodo just another ORM tool?

Well, fundamentally, Commodo is not an ORM tool, but can very quickly become one, by utilizing additional higher order functions, for example, the `withStorage` function, from the [@commodo/fields-storage package](https://github.com/webiny/commodo/tree/next/packages/fields-storage), that we mentioned in our last video. 

```js
import { withStorage } from "@commodo/fields-storage"
```

So, it really depends on how your model is defined or in other words, what it can do.

### Use only what you need
Commodo consists of a couple of base packages, enabling the developer to only import the functionality that’s actually needed. 

```js
import { withFields, string, number, foolean, onSet } from "@commodo/fields";
import { withName } from "@commodo/name";
import { withHooks } from "@commodo/hooks";
import { withStorage } from "@commodo/fields-storage";
```

With this approach, we can define anything from simple models consisting only of a couple of fields:
 
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
 
...to models that represent a more complex data structure, that can also execute custom business logic, and as mentioned, may persist data in a database:

```js
import { withFields, string, number, boolean, fields, onSet } from "@commodo/fields";
import { withName } from "@commodo/name";
import { withHooks } from "@commodo/hooks";
import { withStorage } from "@commodo/fields-storage";
import { MongoDbDriver, withId } from "@commodo/fields-storage-mongodb";
import { compose } from "ramda";

// Define User and Verification models.
const Verification = compose(
  withFields({
    verified: boolean(),
    verifiedOn: string()
  })
)();

const User = compose(
  withFields({
    firstName: string(),
    lastName: string(),
    email: compose(
      onSet(value => value.toLowerCase())
    )(string()),
    age: number(),
    scores: number({ list: true }),
    enabled: boolean({ value: false }),
    verification: fields({ instanceOf: Verification })
  }),
  withHooks({
    async beforeCreate() {
      if (await User.count({ query: { email: this.email } })) {
        throw Error("User with same e-mail already exists.");
      }
    }
  }),
  withName("User"), // Utilized by storage layer, to determine collection / table name.
  withId(),
  withStorage({
    driver: new MongoDbDriver({ database })
  })
)();
```
This flexibility is one of the core ideas of the Commodo library. 
 
 ### Composition over inheritance
Finally, Commodo library is following a concept called composition over inheritance.

In a traditional OOP approach, we often create classes that inherit properties from parent or base classes. 

![Commodo](/img/api-development/commodo/core-ideas/coi-1.png)

While this approach in a lot of cases works just fine, in others it might start to cause some problems. For example, the child classes may eventually start to inherit properties that might not be relevant to them.

![Commodo](/img/api-development/commodo/core-ideas/coi-2.png)

That’s one of the reasons Commodo follows the composition over inheritance approach, in which we facilitate different properties and behaviours with  simple functions, and then compose them in order to create our models.

![Commodo](/img/api-development/commodo/core-ideas/coi-3.png)

For now, don’t worry if some of these concepts sound strange or confusing. We will see how all of this works on practical examples, in the following sections.
