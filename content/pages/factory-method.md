---
author: content/data/nacho.json
colors: bg-light-fg-dark
date: '2023-06-24'
featuredImage:
  altText: Thumbnail
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/factory.jpeg
isFeatured: false
seo:
  metaDescription: 'Factory Method Pattern: Creating Objects Without Knowing Exact
    Types.'
  metaTitle: 'Factory Method Pattern: Creating Objects Without Knowing Exact Types'
  socialImage: /images/factory.jpeg
  type: Seo
slug: factory-method
styles:
  self:
    flexDirection: col
title: 'Factory Method Pattern: Creating Objects Without Knowing Exact Types'
type: PostLayout
---

The Factory Method pattern is a creational design pattern that provides a way to create objects without knowing their exact dependencies and types in advance. Instead of using the "new" operator directly, this pattern encapsulates the object creation process in a method called the factory method.

## Key Components of the Factory Method Pattern

![](/images/factory.png)

1.  **Product Interface**: Make all products follow the same interface. This interface should declare methods that make sense in all products. By adhering to a common interface, the products can be treated uniformly in the client code.
    
2.  **Concrete Products**: Implement the products that meet all the interface methods with consistency. Each concrete product represents a specific implementation of the product interface.
    
3.  **Creator Class**: The Creator class declares the factory method, which returns the interface common to all concrete products. It may or may not be an abstract class. The Creator class encapsulates the object creation and ensures that the client code remains decoupled from the concrete product implementations.
    
4.  **Concrete Creators**: Concrete Creators are subclasses of the Creator class responsible for instantiating the related concrete product. They override the factory method to return specific concrete products.
    


## Conclusion

The Factory Method pattern is a versatile creational design pattern that allows you to create objects without specifying their exact types in advance. By encapsulating the object creation process in a factory method, the pattern promotes loose coupling and flexibility in the client code.

In our cross-platform application example, the Factory Method pattern enables the creation of different buttons for different operating systems, abstracting away the details of button instantiation from the client code. By applying the Factory Method pattern, you can build more maintainable and adaptable systems that can create objects dynamically based on runtime requirements.