---
title: What Is
title-contd: IoC Or DI
permalink: jection/what-is/
layout: page
---

## Stylized Example Components

Lets consider an application scoped component "Inventory", a session scoped component "ShoppingCart", and something 
request-scoped "UpSell". That last is fleeting, and would change for each time it is shown to the user of an 
application. The ShoppingCart is for the user only and may well (subject to implementation) survive logout and login. 
The Inventory is for all users of the application, but isn't something they realize their are interacting with. Indeed
their primary focus is the cart, and the UpSells are appreciated (or annoying) appeals for them to put something 
extra in their cart "People often buy Xyz, when they buy Abc".

Here they are in Java, utilizing constructor injection:

```java
public class Inventory {
  // inventory-centric methods
} 

public class ShoppingCart {
  private final Inventory inventory;
  public ShoppingCart(Inventory inventory) {
     this.inventory = inventory;
  }
  // methods for a shopping cart 
} 

public class UpSell {
  private final ShoppingCart shoppngCart;
  public UpSell(ShoppingCart shoppngCart) {
     this.shoppngCart = shoppngCart;
  }
  // methods for a shopping cart 
} 
```

The container (who's code isn't represented above, does the magic to instantiate them at the appropriate moment (based on need).

Note, that not all DI containers allow for scoped (application, session, request) usage. Some do, but fix the scope. Some 
have more than three scopes, and some have as many scopes as you like and don't tie them to fixed concepts like those. 
Some frameworks call Application-Scope "Singleton" which can confuse people as Singletons (by the Gang of Four definition)
were what IoC and Dependency Injection diametrically oppose.

## Components should be minimal yet idiomatically correct for the language

The code above is **plain Java**. POJO-like (Plain Old Java Objects; ThoughtWorks' Martin Fowler, Rebecca Parsons and 
Josh MacKenzie in 2000, as a reaction to "Enterprise Java Bean" *in your face* specs[![](/ext.png)](https://en.wikipedia.org/wiki/Plain_old_Java_object))) 
is the goal here. POCO for .NET and POPO for Python (and others) followed.

Dependency Injection Containers get as close to that as they can.  The test being how easy it is to use the same component
outside and explicitly without any containers. Say for example in unit tests. And test-related again, how easy it would 
be to mock the collaborators (Mockito and others).

Incidentally, since we mention POJOs, setters and getters are part of plain Java. Same as properties for .NET.

### Components with a smidgen of framework/container

```
public class ShoppingCart {
  private Inventory inventory;
  public void injectDependency(Inventory inventory) {
     this.inventory = inventory;
  }
  // methods for a shopping cart 
} 
```

A method called `injectDependency` might be what a hypotherical framework or container mandates for dependency injection
but this is still very close to setters, and can certainly be used in a unit test without boilerplate, so we'll 
give it a pass and say that DI containers may specify components this way. Annotations like Java's @Inject (defined 
later as a standard) are the same.

### Components with a lot of framework in them

```
import fooframework.Injektor
public class ShoppingCart {
  private final Inventory inventory;
  public ShoppingCart(Injektor injektor) {
       this.inventory = injektor.injekt(Inventory.class);
  }
  // methods for a shopping cart 
} 

// or
import fooframework.Injektor
public class ShoppingCart {
  private Inventory inventory;
  public void doInjections(Injektor injektor) {
       this.inventory = injektor.injekt(Inventory.class);
  }
  // methods for a shopping cart 
} 

```

We've stepped away from Dependency Injection here and potentially into the sphere of "Inversion of Control" (IoC). Still a million 
times better than ServiceLocator of classic GoF Singleton, but this was the style of the frameworks that came before Martin 
Fowler's industry-changing "Inversion of Control Containers and the Dependency Injection pattern" article.

There are some caveats though: Injektor in the hypothetical container must have prior knowledge of what ShoppingCart 
needs. It cannot simply allow any appropriately-scoped component to be passed in via the `injekt` method. Older frameworks like 
Apache's Avalon had an XML manifest that the framework would process before attempting to instantiate the component. 
Indeed, exceptions could be deliberatrly throws before the attempt to instantiate the component based on missing 
depeendencies.  

