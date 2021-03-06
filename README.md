# common-scala-test

Contains a number of useful extensions of ScalaTest and Mockito, making it easier and more convenient to test your Scala code.

## Using MockitoSyrup

As ScalaTest contains the `MockitoSugar` trait that contains adaptations for using Mockito with Scala code, we have added our extensions in a trait `MokcitoSyrup` - making your tests even sweeter! To use it, simply mix it in with your test suites instead of `MockitoSugar`.

The trait itself is a combination of more specific traits, that you can use individually if you so wish. The following sections will describe in more detail what's available in each of these.

## AnswerSugar

The `AnswerSugar` trait makes it easier to work with Mockito `Answer`s in Scala. Instead of creating an `Answer[T]` object and overriding `answer()`, you can pass in a closure to the method. In other words, instead of writing:

```scala
when(myMock.method()).thenAnswer(new Answer[ReturnType] {
  override def answer(invocation: InvocationOnMock): ReturnType = "My result"
}
```

you can write:

```scala
when(myMock.method()).thenAnswer(() => "My result")
```

## MatcherSugar

The `MatcherSugar` trait allows you to write `Matcher`s as anonymous functions in Scala. Instead of creating a `Matcher[T]` object and overriding `matches`/`describeTo`, you can pass in a closure to the method. In other words, instead of writing:

```scala
verify(myMock).method(argThat(new Matcher[String] {
  override def matches(item: Any): Boolean = item == "test string"
  override def describeTo(description: Description): Unit = description.appendText("test string")
}))
 ```

you can write:

```scala
verify(myMock).method(argThat{ arg: String => arg == "test string" })
```

There's an additional feature of `MatcherSugar` that deals with a small annoyance: the name `eq` method defined in Mockito's `Matchers` class clashes with Scala's built-in `eq` function (which compares for object identity). This means you can't just import `org.mockito.Matchers.eq`, you have to explicitly prefix it wherever it's used, making test code a little bit more verbose than we like.

`MatcherSugar` gets around this simply by defining a method `eql` which just delegates to `org.mockito.Matchers.eq()`. So you can use `eql` as a synonym for `eq` when using matchers, for example:

```scala
verify(myMock).method(eql(42), anyString())
```
