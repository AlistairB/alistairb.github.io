---
layout: post
title: "Haskell ADT Rules And When To Break Them"
tags: [sample post, code, highlighting]
comments: true
---

ADTs are a powerful tool for structering data. There are an array of options and Haskell provides many inbuilt data types we can leverage. However, picking between these options can be tricky. Different solutions have different benefits.

First let's look at what we hope to get out of our structured types.

## What we want

### Type safety

Naievly we may make all our types use `String`.

```haskell
data Person = Person {
  name :: String,
  address :: String
}
```

But this means we can easily switch around `name` and `address` and the GHC won't help us.

```haskell
newtype PersonName = PersonName String
newtype PersonAddress = PersonAddress String

data Person = Person {
  name :: PersonName,
  address :: PersonAddress
}
```

In this version GHC will prevent us from switching them around. Similary we can write functions that operate on `PersonName` and again we can't pass in any old `String`.

### Ease Of Use

On the flip side, creating a `Person` now becomes more cumbersome.

```haskell
joanna :: Person
joanna = Person (PersonName "Joanna") (PersonAddress "1 blah st")
```

Ok not too bad really. However, we can take the type
