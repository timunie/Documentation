# Avalonia Concepts Explained by Easy Samples

## Introduction

This is a hands on guide to the Avalonia \(and WPF\) concepts providing C\# samples which highlight each concept. I recommend that you run all the samples and also try to create similar samples by yourself in order to completely master the Avalonia concepts.

## Avalonia XAML Basics

### What is XAML \(Introduction\) 

XAML is XML used for building C\# \(mostly visual\) objects.

 C\# classes are displayed as XML tags, while class properties are usually displayed as XML attributes:

```markup
<my_namespace:MyClass Prop1="Hello World"
                      Prop2="123"/>
```

The XAML code in the example above creates an object of type `MyClass` from XML namespace `my_namespace` and sets its properties `Prop1` to string "Hello World" and `Prop2` to value `123`. Note that the properties will be resolved to their C\# type, e.g. if `Prop2`is of `int` type, it will be resolved to `123` integer value, while if it is of `string` type, it will be resolved to `"123"` string. If the property or type mentioned in XAML file do not exist, the compilation of the project that contains that XAML will fail and often the Visual Studio will detect an error even before the compilation and will underscore the missing type or property with a red broken line.

The namespace \(in our sample it is "my\_namespace"\) should usually be defined above or within the XML tag where it is used. It can point to a C\# namespace or a set of C\# namespaces \(as will be explained below with an appropriate example\).

XAML file can be associated with a C\# file called "code-behind" to define the same class using "partial class" declarations. The C\# code-behind usually contains definitions of methods that serve as event handlers for the events fired by elements defined in XAML file. This way of associating events fired by XAML elements and C\# event handlers is the easies and most straightforward, but is also the worst, since it breaks the important MVVM pattern \(as will be shown later\) and should almost never be used.

XAML is almost XML, but compiled. XML tags are classes. Attributes are properties. For properties that are complex types \(and need to be assembled - use Element.Property notation\)

Explain the namespaces, 

Basic Markup extensions: x:Static and StaticResource with visual samples

Referencing Assets \(e.g. images\).

XML Resources

Pulling resources into C\# code.

## Visual Trees

## Logical Trees

## Avalonia Special Properties

## Avalonia Bindings

## Routed Events

## Commands, Calling C\# Methods from XAML

## Avalonia User Controls - do not build them - they are the worst

## Avalonia ControlTemplates and CustomControls

## MVVM Pattern, `DataTemplates`, `ItemsPresenter` and `ContentPresenter`

## Styles

## Animations

## 





