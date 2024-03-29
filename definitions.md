---
title: Definitions
permalink: /jection/definitions/
layout: page
---

## Library vs Framework

A library is something you can use, and stop using within an programming scope, and their usage feels close to the 
idioms of the language. A framework takes over some aspect of your application, and might not relinquish that until 
the process ends. Frameworks change the way you code, to various degrees, and have moved away from idioms for the 
language in question.

In the context of Inversion of Control and Dependency Injection containers, Library vs Framework is a sliding scale.

There's a famous answer to the same question on stack overflow <a href="https://stackoverflow.com/questions/148747/what-is-the-difference-between-a-framework-and-a-library/233765#233765"><svg style="display: color=blue; inline-block; width: 1em; height: 1em;" viewbox="0 0 48 48"><path d="M36 24c-1.2 0-2 0.8-2 2v12c0 1.2-0.8 2-2 2h-22c-1.2 0-2-0.8-2-2v-22c0-1.2 0.8-2 2-2h12c1.2 0 2-0.8 2-2s-0.8-2-2-2h-12c-3.4 0-6 2.6-6 6v22c0 3.4 2.6 6 6 6h22c3.4 0 6-2.6 6-6v-12c0-1.2-0.8-2-2-2z"></path><path d="M43.8 5.2c-0.2-0.4-0.6-0.8-1-1-0.2-0.2-0.6-0.2-0.8-0.2h-12c-1.2 0-2 0.8-2 2s0.8 2 2 2h7.2l-18.6 18.6c-0.8 0.8-0.8 2 0 2.8 0.4 0.4 0.8 0.6 1.4 0.6s1-0.2 1.4-0.6l18.6-18.6v7.2c0 1.2 0.8 2 2 2s2-0.8 2-2v-12c0-0.2 0-0.6-0.2-0.8z"></path></svg></a> with a terrible Japanese poem to add drama <a href="https://meta.stackoverflow.com/questions/345434/popular-answer-saying-that-helping-the-library-hurts-many-tears-a-japanese-poem"><svg style="display: color=blue; inline-block; width: 1em; height: 1em;" viewbox="0 0 48 48"><path d="M36 24c-1.2 0-2 0.8-2 2v12c0 1.2-0.8 2-2 2h-22c-1.2 0-2-0.8-2-2v-22c0-1.2 0.8-2 2-2h12c1.2 0 2-0.8 2-2s-0.8-2-2-2h-12c-3.4 0-6 2.6-6 6v22c0 3.4 2.6 6 6 6h22c3.4 0 6-2.6 6-6v-12c0-1.2-0.8-2-2-2z"></path><path d="M43.8 5.2c-0.2-0.4-0.6-0.8-1-1-0.2-0.2-0.6-0.2-0.8-0.2h-12c-1.2 0-2 0.8-2 2s0.8 2 2 2h7.2l-18.6 18.6c-0.8 0.8-0.8 2 0 2.8 0.4 0.4 0.8 0.6 1.4 0.6s1-0.2 1.4-0.6l18.6-18.6v7.2c0 1.2 0.8 2 2 2s2-0.8 2-2v-12c0-0.2 0-0.6-0.2-0.8z"></path></svg></a>:

![](/148747.png)

## Components

An aggregation of classes (or more ambiguous modules from non-OO languages) that suits a larger purpose that’s closer 
to a business description. “ShoppingCart” and “Inventory” are good examples. In OO, as well as being an aggregation 
of classes, components are also classes themselves. Components are instantiated in a context too, and that could be 
a container. There’s an implied pluggability to components in this model. Terms like decomposition and 
componentization apply (they mean the same thing) when considering refactoring work to make a better code base.

## Containers 

At the simplest definition specialization of ‘library’. Software that hosts multiple guest software 
things. Host and guest; container and contained. There’s always an API between the container and contained. 
Containers control the life-cycle of the things they contain. Contained things are nearly always lesser capable 
than the container they are in - a sandbox concept. Contained things can only invoke functionality in the container 
itself if the container provides an API to do so. In some cases, the contained thing may not explicitly know it 
is contained. Windows on VmWare is an example, or should be, and as a container concept itself is way beyond a 
simple ‘specialization of library’. If it was not clear, containers should generally include the ability to have 
more fine-grained containers used within their contained components, but do not always. Presently Docker, as a 
container, does not allow additional embedded Docker containers within it. I hope that changes in time and Docker 
‘compose’ is a workaround I think. Components being added to running containers should not require a restart of 
the container seems like a requirement to me, but again is not always facilitated. Self-reference on that and 
an elaboration of many container/contained things: [principles-of-containment](https://paulhammant.com/2016/12/14/principles-of-containment/).

## Singletons

This can be confusing in the field of Inversion of Control and Dependency Injection - we want to avoid the Singleton 
design pattern (anti-pattern), but are OK with Singletons that are idiomatic to the IoC/DI container in question.

### 1994 definition in the Design Patterns book.

The book "Design Patterns" was written Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides (the Gang of Four), 
in 1994. See the Wikipedia page for the book <a href="https://en.wikipedia.org/wiki/Design_Patterns"><svg style="display: color=blue; inline-block; width: 1em; height: 1em;" viewbox="0 0 48 48"><path d="M36 24c-1.2 0-2 0.8-2 2v12c0 1.2-0.8 2-2 2h-22c-1.2 0-2-0.8-2-2v-22c0-1.2 0.8-2 2-2h12c1.2 0 2-0.8 2-2s-0.8-2-2-2h-12c-3.4 0-6 2.6-6 6v22c0 3.4 2.6 6 6 6h22c3.4 0 6-2.6 6-6v-12c0-1.2-0.8-2-2-2z"></path><path d="M43.8 5.2c-0.2-0.4-0.6-0.8-1-1-0.2-0.2-0.6-0.2-0.8-0.2h-12c-1.2 0-2 0.8-2 2s0.8 2 2 2h7.2l-18.6 18.6c-0.8 0.8-0.8 2 0 2.8 0.4 0.4 0.8 0.6 1.4 0.6s1-0.2 1.4-0.6l18.6-18.6v7.2c0 1.2 0.8 2 2 2s2-0.8 2-2v-12c0-0.2 0-0.6-0.2-0.8z"></path></svg></a>. 
A Java example:

```java
public final class Inventory {

    private static final Inventory INSTANCE = new Inventory();

    private Inventory() {}

    public static Inventory getInstance() {
        return INSTANCE;
    }
}
```

We'll refer to this as **GoF Singletons** to differentiate in this site.

### Use as idiom since 2002

If your framework is responsible for instantiation of all components, then classes could be marked as "singleton" 
somehow in order to indicate once instance for the container. A Java example:

```java
@Singleton
public final class Inventory {

    public Inventory() {}
}
```

## Service Locators

Another anti-pattern, ServiceLocator use sits in opposition to Inversion of Control and Dependency Injection. Or
rather Inversion of Control and Dependency Injection, coming later, sit in opposition to ServiceLocator (as well as 
GoF Singletons) Wikipedia has a page on ServiceLocator <a href="https://en.wikipedia.org/wiki/Service_locator_pattern"><svg style="display: color=blue; inline-block; width: 1em; height: 1em;" viewbox="0 0 48 48"><path d="M36 24c-1.2 0-2 0.8-2 2v12c0 1.2-0.8 2-2 2h-22c-1.2 0-2-0.8-2-2v-22c0-1.2 0.8-2 2-2h12c1.2 0 2-0.8 2-2s-0.8-2-2-2h-12c-3.4 0-6 2.6-6 6v22c0 3.4 2.6 6 6 6h22c3.4 0 6-2.6 6-6v-12c0-1.2-0.8-2-2-2z"></path><path d="M43.8 5.2c-0.2-0.4-0.6-0.8-1-1-0.2-0.2-0.6-0.2-0.8-0.2h-12c-1.2 0-2 0.8-2 2s0.8 2 2 2h7.2l-18.6 18.6c-0.8 0.8-0.8 2 0 2.8 0.4 0.4 0.8 0.6 1.4 0.6s1-0.2 1.4-0.6l18.6-18.6v7.2c0 1.2 0.8 2 2 2s2-0.8 2-2v-12c0-0.2 0-0.6-0.2-0.8z"></path></svg></a> 

A Java example:

```java
public final class MyServiceLocator {

    private static final MyServiceLocator INSTANCE = new MyServiceLocator();

    private MyServiceLocator() {}

    public static MyServiceLocator getInstance() {
        return INSTANCE;
    }

    public void put(Class type, Object implementation) { /* magic */ }
    public Object get(Class type) { /* magic */ }
}
```

That's right, it is a map-like GoF Singleton that can hold component, and make them globally available to anything that 
can do `MyServiceLocator.getInstance()`. Note that the code above is overly simple, devoid of implementation detail,
and not even using Java 5's generics.



