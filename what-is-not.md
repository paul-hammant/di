---
title: What Is Not
title-contd: IoC Or DI
permalink: /jection/what-is-not/
layout: page
---

## Singletons and Service Locators

Singleton (per the Gang of Four Design patterns book) containers are not DI or IoC at all they are really 
ServiceLocators:

```java
Container container = Container.INSTANCE()
container.add(FooType, FooTypeImpl)
container.add(BarType, BarTypeImplNeedsFoo)
BarType bar = container.get(BarType)
```

`Container.getInstance().add(..)`, `Container.INSTANCE.add(..)` and alike, too.

## Dependency Injection with a ServiceLocator back door

If you make secondary ServiceLocator (the design patten) way of accessing 
your DI container, you've opened pandora's box. Downstream from, you the creator of the DI container, there are
enterprise teams scaling up a multi-component development team. 

If such teams are relying on the ServiceLocator
way of accessing components (globally accessible static map/dictionary components), then they shouldn't report
that they are doing DI at all.

If such teams are reluctantly using the ServiceLocator backdoor, then they should keenly police all commits that use 
it to ensure that the larger project isn't transitioning to a hairball, and away from the tenets of DI and IoC.

One such example is Java's Servlet spec. Originally made in 1997 or so, it gained features over the years. One such feature
"taglets" were a snipped of Java that would be instantiated server-side in conjunction with the rendering of HTML for a 
small section of the web UI. Say a single 'div'. That instantiation was done by the webserver (say Tomcat), and if you 
wanted to do constructor-injection for that class, you were out of luck - the Servlet spec didn't support that.

The excellent SpringFramework team had to engineer a lesser-used way of accessing the container from a taglet's Java code. [WebApplicationContextUtils](https://github.com/spring-projects/spring-framework/blob/master/spring-web/src/main/java/org/springframework/web/context/support/WebApplicationContextUtils.java) was their solution. Code like this was then possible:

```
PersistenceStore ps = (PersistenceStore) WebApplicationContextUtils
    .getRequiredWebApplicationContext(servletContext).getBean("PersistenceStore");
```

Sure, you need to pass in a reference to a `ServletContext`, but that's not hard for arbitrary servlets, filters, liseners, 
taglets. Regular things you make if you're developing for vanilla servlets-spec solutions.

So Spring isn't consigned to the the realms of "not a DI container", but teams should grep for `getRequiredWebApplicationContext` 
and have tough conversations, as the codebase grows. 

As it happens the SpringBoot way of working doesn't require you to write the likes of taglets, so modern Spring solutions 
never encounter this.