---
title: Types
title-contd: Of DI & IoC
permalink: /jection/types/
layout: page
---

This page attempt to define subtypes for IoC and Dependency Injection.

IoC frameworks have more that just dependency resolution in their capabilities, but it is all that we 
will dwell on here. 

### Contextualized lookup IoC

Inversion of Control (that came before DI) was implemented as a framework. Here's a stylized example (Java):

```java
public class MyShoppingCart {
  private final Inventory inventory;
  public MyShoppingCart(ScopedDependencyLookupMechanism sdlm) {
    inventory = sdlm.lookup(Inventory.class);
  }
  // methods that would use inventory (or not)
}

// or as it really was for Apache's Avalon 1998-2004.
public class MyShoppingCart implements Serviceable {
  private Inventory inventory;
              
  public void service(ServiceManager manager) throws ServiceException {
    inventory = (Inventory) manager.lookup("inventory");
  } 
  // methods that would use inventory (or not)
}
```

Apache's Avalon framework had more lines of code than that and did not use constructors, but the intention was the same. Specifically, `ScopedDependencyLookupMechanism` is a stylized interface that never really existed, but captures the intent in its name.  The 
framework would throw and exception if you attempted to lookup a dependency that had not explicitly been declared in metadata. 
Avalon's metadata was XML that was bundled with the compiled classes.

Apache's Avalon was the first implementation of the IoC idea, and Phoenix & Cocoon the first containers

## Some History.

"Inversion of Control" was a Ralph Johnson & Brian Foote coin (1998), in [Designing Reusable Classes (JOOP)](http://www.laputan.org/drc/drc.html). Martin Fowler goes into the history [here](https://martinfowler.com/bliki/InversionOfControl.html). 

Others involved with pushing towards framework/containers were Michael Mattesson (1996) [Object-Oriented Frameworks : A survey of methodological issues](http://web.archive.org/web/20000815083810/www.ipd.hk-r.se/michaelm/thesis/toc.html) (“inversion of control” used in passing) and Steffano Mazzocchi pushing Apache's Avalon from 1998 (project shut down in 2004). 

Other names for the same thing, or the an overlapping field of science: “Hollywood Pattern” (don’t call us, we’ll call you) (In the GoF Design Patterns book), Bob Martin's “OO Design Quality Metrics: An Analysis of Dependencies” [posting](http://groups.google.com/group/comp.lang.c++/msg/b9c929f55c52e4a2) (on the comp.lang.c++ newsgroup, August 1994). Also “The Principles of OOD” in [posting](http://groups.google.com/group/comp.lang.c++/msg/30f7c7701209faba?dmode=source) (on the comp.lang.c++, June/July 1995). Also "The Dependency Inversion Principle" [article](http://www.objectmentor.com/resources/articles/dip.pdf) (in C++ Report, Vol. 8, May 1996). From the 2000's The D is SOLID that Bob is associated with is in the same space.

### DI: Constructor Injection

Contrast to DI the constructor way (early days before annotations), per PicoContainer (Java):

```java
public class MyShoppingCart {
  private final Inventory inventory;
  public void MyShoppingCart(Inventory inventory) {
    this.inventory = inventory;
  }
  // methods that would use inventory (or not)
}
```

Clearly this is plain Java and could be instantiated with or without a container/framework. The first generation of DI containers would 
instantiate this using reflection, and allow for a variable number of dependencies in the constructor.

### DI: Setter Injection

Contrast to DI the setter way (early days DI before annotations), per SpringFramework (java):

```java
class MyShoppingCart {
  private final Inventory inventory;
  public void MyShoppingCart() {
  }
  public void setInventory(Inventory inventory) {
    this.inventory = inventory;
  }
  // methods that would use inventory (or not)
}
```

Containers and frameworks could implement this with or without meta-data (pros and cons to both). This is generally felt to inferior to constructor injection.

### DI: Field Injection

Contrast to DI the setter way (early days DI before annotations), per SpringFramework (java):

```java
class MyShoppingCart {
  // framework ignores the 'private' scope and sets the field after instantiation.
  private final Inventory inventory;
  public void MyShoppingCart() {
  }
  // methods that would use inventory (or not)
}
```

Containers and frameworks could implement this with or without meta-data (pros and cons to both again). This is generally felt to inferior to constructor injection and setter injection - principally because it is not able to be instantiated and dependency-injected in plain Java.

## Later generations of DI

At the beginning DI containers used the language's basic idioms: constructors have signatures, setters conform to a naming convention. The containers used the languages reflection capability to interact with those during instantiation without explicit lines of Java.

With Java 5, annotations could be used to supply meta-data to various degrees. @Inject (in front of the constructor, setter or field) would become standardized for "this dependency needs to be injected for the component to work". Annotations like this didn't impede the ability for the component to be instantiated outside the container.

