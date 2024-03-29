---
layout: post
title:  "Enumeratum"
date:   2024-03-18 11:00:01 +0200
tags:   [scala, enumeratum]
---

Scala has an amazing library do deal with enums: `enumeratum`

`libraryDependencies += "com.beachape" %% "enumeratum" % "1.7.3"`

it gives great declarative enum naming:

```scala mdoc
import enumeratum.EnumEntry._
import enumeratum._

sealed trait Color extends EnumEntry with Lowercase
object Color extends Enum[Color] {
  override def values: IndexedSeq[Color] = findValues
  case object Red extends Color
  case object Green extends Color
}
```
By mixing `Lowercase` it automatically provides name transformation based on the object name
and the `Lowercase` rule
```scala mdoc
val c1 = Color.Red.entryName
val c2 = Color.Green.entryName
```
By changing `Lowercase` to `Uppercase`
we will get another name transformer and name as a result
```scala
sealed trait Color extends EnumEntry with Uppercase
```
and get
```scala
val c1 = Color.Red.entryName
// c1: String = "RED"
val c2 = Color.Green.entryName
// c2: String = "GREEN"
```
but what if, maybe due to some weird requirements, maybe due to the kind of legacy,
you need to have `red` and `GREEN`?
Library copes with that elegantly!
Simply override method `entryName`
```scala mdoc
import enumeratum.EnumEntry._
import enumeratum._

sealed trait Color2 extends EnumEntry with Lowercase
object Color2 extends Enum[Color2] {
  override def values: IndexedSeq[Color2] = findValues
  case object Red extends Color2
  case object Green extends Color2 {
    override def entryName: String = "GREEN"
  }
}
```
`Lowercase` rule will be applied to all elements
but to other, for which we need another rule, we simply override `withName` method.
And we get
```scala mdoc
val c3 = Color2.Red.entryName
val c4 = Color2.Green.entryName
```
