---
author: Ignacio Lopez
colors: bg-light-fg-dark
date: '2023-06-02'
featuredImage:
  altText: Thumbnail
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/pillars.jpeg
isFeatured: false
seo:
  metaDescription: 'Exploring the Core Principles of Object-Oriented Programming:
    The Pillars of OOP.'
  metaTitle: 'Exploring the Core Principles of Object-Oriented Programming: The Pillars
    of OOP'
  socialImage: /images/pillars.jpeg
  type: Seo
slug: oop-pilars
styles:
  self:
    flexDirection: col
tags:
- design-patterns
- oop
title: 'Exploring the Core Principles of Object-Oriented Programming: The Pillars
  of OOP'
type: PostLayout
---

# OOP Pillars

The object-oriented programming paradigm has four basic principles known as "**the four pillars of OOP**" that we must adhere to:

1.  **Abstraction**: It involves representing real-world objects in classes by including only the relevant attributes and methods. We don't need to represent 100% of the object's attributes and actions unless we use them.
    
    Example: Representation of an airplane.
    
    ![Abstraction Example](h./../images/abstraction.png)
    
2.  **Encapsulation**: It helps minimize the impact of code changes by grouping related data and behavior into a single unit, known as a class. It involves hiding internal implementation details and providing controlled access through methods.
    
    Example: Avoiding a common mistake in an e-commerce system where order calculation and tax calculation are performed together in the same method.
    
    ![Encapsulation Example](./images/encapsulation.png)
    
    Encapsulation can be achieved by encapsulating tax calculation in a private method and separating it into a new class, thereby improving code organization.
    
    ![Encapsulation Example](./images/encapsulation-example.png)
    
3.  **Polymorphism**: It allows objects of different classes to be treated as objects of a common superclass. Polymorphism enables programming to an interface rather than an implementation. It promotes code flexibility and reusability.
    
    Example: Demonstrating the concept of polymorphism by having a cat with a dependency on food. By programming to an interface, the cat's energy can be improved by changing its diet, even though the specific type of food can vary.
    
    ![Polymorphism Example](./images/polyformism.png)
    
4.  **Inheritance**: It enables the creation of new classes based on existing classes, promoting code reuse. Inheritance allows subclasses to inherit properties and methods from their parent class. However, subclasses must implement all abstract methods defined in the parent class.
    
    Example: Illustrating inheritance by creating new classes on top of existing classes, leading to code reuse.
    
    ![Inheritance Example](./images/inheritance-example.png)
    

By adhering to these four pillars of OOP, we can design and implement robust and maintainable object-oriented systems.