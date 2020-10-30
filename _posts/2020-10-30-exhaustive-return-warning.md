---
layout: post
title: "Can We Have Exhaustive Return Warning?"
tags: [haskell]
comments: true
---

## Sum Types Are Cool

One of the simplest and coolest features of Haskell is sum types. A common usage of sum types is to represent stringly typed data in a structured and type safe format. We might define such a type.

```haskell
data Color =
    Red
  | Green
  | Blue
```

We can then pattern match on this type to convert from our sum type into some other format.


```haskell
colorToText :: Color -> Text
colorToText Red   = "red"
colorToText Green = "green"
colorToText Blue  = "blue"
```

This is all well and good, but we need to be careful that we have covered all the cases in the pattern match. For example, if we add a new color `Purple`, the function if not updated will be partial and throw an error on `colorToText Purple.` Thankfully with `-Wall` and `-Werror` enabled the original function will fail to compile as the pattern match is non-exhaustive.

## Exhaustive.. Return Value?

This is all very cool, however what about going the other direction, from text to color?

```haskell
textToColor :: Text -> Maybe Color
textToColor "red"   = Just Red
textToColor "green" = Just Green
textToColor "blue"  = Just Blue
textToColor _       = Nothing
```

A function like this will also need to be updated, however in this case the compiler won't help us. We are pattern matching on strings and as we have a catch all case of `_` the pattern match is still exhaustive.

For this function we want a way to say the return value is exhaustive. Which is to say all possible combinations of data constructors should be used in one of the pattern matches.

Now this property won't hold for many functions. We expect to use all data constructors for sum types somewhere in a program, but not necessarily for every function. As such, it would need to be opt in somehow. eg.

```haskell
isOne :: Int -> Bool
isOne 1 = True
isOne _ = False
{-# EXHAUSTIVE_RETURN isOne #-}
```

With such a feature in place we could have `textToColor` emit a warning when we add `Purple`.

Now, I have no idea how difficult this might be to implement. My guess is 'quite' difficult. I suspect it would also need to have clear limitations such as only supporting simple pattern match to data constructor type functions. Complex further calculation in the function body would surely make it a lot more difficult to determine. ie.

```haskell
-- presumably too complex
isOneOrGreaterThan17 :: Int -> Bool
isOneOrGreaterThan17 1 = True
isOneOrGreaterThan17 x =
  if x > 17
    then True
    else False
{-# EXHAUSTIVE_RETURN isOneOrGreaterThan17 #-}
```

Also, would it allow the same data constructor combination to be used twice? You may or may not want that depending on the situation. For `textToColor` we probably don't want to allow this.

## Can Haz?

Would this feature be valuable to people?

And is it feasible (without huge effort) to implement in GHC or as a GHC plugin?
