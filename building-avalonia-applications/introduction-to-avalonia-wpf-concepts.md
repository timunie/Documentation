# Avalonia Concepts Explained by Easy Samples

## Introduction

This is a hands on guide to the Avalonia \(and WPF\) concepts providing C\# samples which highlight each concept. I recommend that you run all the samples and also try to create similar samples by yourself in order to completely master the Avalonia concepts.

## Avalonia XAML Basics in Samples

### Introduction to XAML 

XAML is XML used for building C\# \(mostly visual\) objects.

 C\# classes are displayed as XML tags, while class properties are usually displayed as XML attributes:

```markup
<my_namespace:MyClass Prop1="Hello World"
                      Prop2="123"/>
```

The XAML code in the example above creates an object of type `MyClass` from XML namespace `my_namespace` and sets its properties `Prop1` to string "Hello World" and `Prop2` to value `123`. Note that the properties will be resolved to their C\# type, e.g. if `Prop2`is of `int` type, it will be resolved to `123` integer value, while if it is of `string` type, it will be resolved to `"123"` string. If the property or type mentioned in XAML file do not exist, the compilation of the project that contains that XAML will fail and often the Visual Studio will detect an error even before the compilation and will underscore the missing type or property with a red broken line.

The namespace \(in our sample it is "my\_namespace"\) should usually be defined above or within the XML tag where it is used. It can point to a C\# namespace or a set of C\# namespaces \(as will be explained below with an appropriate example\).

XAML file can be associated with a C\# file called "code-behind" to define the same class using "partial class" declarations. The C\# code-behind usually contains definitions of methods that serve as event handlers for the events fired by elements defined in XAML file. This way of associating events fired by XAML elements and C\# event handlers is the easies and most straightforward and was already explained in [Creating and Running a Simple Avalonia Application under Visual Studio sample](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/#creating-and-running-a-simple-avalonia-application-under-visual-studio) section, but is also the worst, since it breaks the important MVVM pattern \(as will be shown later\) and should almost never be used.

In the rest of this chapter, we shall give simple samples to explain various Avalonia XAML features.

The code for all XAML samples shown below is located within [NP.Avalonia.Demos](https://github.com/npolyak/NP.Avalonia.Demos/) repository under its [NP.Demos.XamlSamples](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples) folder. Some of the XAML advanced concepts \(e.g. attached properties, templates and styles\) will only be mentioned briefly in this chapter, while their detailed explanation will be given in the section describing the concept itself.

### XAML Namespaces Sample

XAML namespace is a string usually defined at the top level element of the XAML file \(event though it can be defined on any tag\) and pointing to some C\# namespace\(s\) within some .NET assembly or assemblies that the project containing the current XAML file is dependent on. 

Take a look at the top two lines of [MainWindow.xaml](https://github.com/npolyak/NP.CodeForAvaloniaInEasySampleArticle/blob/main/NP.Demos.SimpleAvaloniaProject/MainWindow.axaml) file of our introductory sample:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" ... >
```

These two lines define two XAML namespaces for whatever is within the file. One of these namespaces which does not require any prefix \(it has an empty prefix\) and the other has prefix "x". Both namespaces refer to the types defined in Avalonia packages. You can define, many elements, \(say a button\) in Avalonia without any prefix \(e.g. as `<Button .../>` \) because those elements are located in the default Avalonia namespace referred to by "https://github.com/avaloniaui" URL. The namespace that is referred to by prefix "x" contains various types that are used slightly less frequently. For example - many C\# built in types  e.g. string and object can be referred to in XAML as `<x:String>...</x:String>` and `<x:Object>...</x:Object>` \(they are contained in the Avalonia namespace referred to by "http://schemas.microsoft.com/winfx/2006/xaml" url\). 

**Note**: the so called XAML namespace URLs do **not** have to refer to any valid URL that really exists on a web and the computer does **not** have to be online in order for them to work. 

The example showing various ways to define custom XAML namespace is located under [NP.Demos.XamlNamespacesSample.sln](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.XamlNamespacesSample) solution. Download its code from github and open the solution in Visual Studio \(or Rider\). 

You can see that the solution consists of 3 projects: the main project NP.Demos.XamlNamespacesSample and two projects on which the main project depends: Dependency1Proj and Dependency2Proj:

![](../.gitbook/assets/image.png)

Compile and run the solution - here is what you are going to see:

![](../.gitbook/assets/image%20%2815%29.png)

There are 5 square buttons of different colors stacked vertically within a window. 

Here is the relevant code of MainWindow.xaml file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:dep1="clr-namespace:Dependency1Proj;assembly=Dependency1Proj"
        xmlns:dep1_sub_Folder="clr-namespace:Dependency1Proj.SubFolder;assembly=Dependency1Proj"
        xmlns:local="clr-namespace:NP.Demos.XamlNamespacesSample"
        xmlns:dep2="https://avaloniademos.com/xaml"
        ...>
  <StackPanel HorizontalAlignment="Center"
              VerticalAlignment="Center">
    <dep1:RedButton/>
    <dep1_sub_Folder:BlueButton/>
    <local:BrownButton/>
    <dep2:GreenButton/>
    <dep2:GrayButton/>
  </StackPanel>
</Window>

```

There are four custom namespaces defined by the following lines of the top XML Tag:

```markup
        xmlns:dep1="clr-namespace:Dependency1Proj;assembly=Dependency1Proj"
        xmlns:dep1_test_Folder="clr-namespace:Dependency1Proj.SubFolder;assembly=Dependency1Proj"
        xmlns:local="clr-namespace:NP.Demos.XamlNamespacesSample"
        xmlns:dep2="https://avaloniademos.com/xaml"
```

Different buttons are are referred two by the corresponding XAML namespace prefixes. Let us take a look at `<RedButton/>` `RedButton` class is defined as a C\# class under Dependency1Proj project. Here is its code:

```csharp
...
namespace Dependency1Proj
{
    public class RedButton : Button, IStyleable
    {
        Type IStyleable.StyleKey => typeof(Button);

        public RedButton()
        {
            Background = new SolidColorBrush(Colors.Red);
            Width = 30;
            Height = 30;
        }
    }
}
```

The line

```csharp
Type IStyleable.StyleKey => typeof(Button);
```

\(and the fact that `RedButton` class implements `IStyleable` interface\) ensures that the default button Styles from the main theme will also be applied to the class `RedButton` which derives from Avalonia `Button` class. The constructor of the button assigns the button color to red and sets the button to have height and width of 30 generic pixels.

Note that the code for each of the buttons of the sample is exactly the same as that for `RedButton` aside from the button class name, C\# namespace and the color assigned to the button.

Now take a look at the line that defines the XAML namespace prefix `dep1` by which we refer to this button inside the XAML file:

```markup
xmlns:dep1="clr-namespace:Dependency1Proj;assembly=Dependency1Proj"
```

The value of the namespace contains two parts divided by ';' semicolon. The first part refers to the C\# namespace: 

```markup
clr-namespace:Dependency1Proj
```

and the second part refers to assembly name:

```markup
assembly=Dependency1Proj
```

In case of `RedButton` both the namespace and assembly name have the same name: `Dependency1Proj`.

`BlueButton` is defined within the same project \(Dependency1Proj\), but within SubFolder folder. Its C\# namespace is not `Dependency1Proj` \(as for the `RedButton`\) but `Dependency1Proj.SubFolder`.

Here is the line that defines the XAML namespace prefix `dep1_sub_Folder` by which `BlueButton` is referred to in MainWindow.xaml file:

```markup
xmlns:dep1_sub_Folder="clr-namespace:Dependency1Proj.SubFolder;assembly=Dependency1Proj"
```

The clr-namespace changed to be `Dependency1Proj.SubFolder` while the assembly part stated the same since `BlueButton`was defined in the same Dependency1Proj assembly.

XAML is almost XML, but compiled. XML tags are classes. Attributes are properties. For properties that are complex types \(and need to be assembled - use Element.Property notation\).

Now take a look at `<local:BrownButton\>` . C\# code for `BrownButton` is defined in the main project NP.Demost.XamlNamespacesSample - the same project that our MainWindow.xaml file is located in. Because of that we can skip the assembly name when defining the prefix "local" \(by which our `BrownButton` is referred to\) and only specify the clr-namespace part:

```markup
xmlns:local="clr-namespace:NP.Demos.XamlNamespacesSample"
```

`GreenButton` and `GrayButton` are defined in two different namespaces of Dependency2Proj. `GreenButton` is defined under the project's main namespace - `Dependency2Proj` while `GrayButton` is defined under `Dependency2Proj.SubFolder` namespace. However, Dependency2Proj also has file AssemblyInfo.cs which defines assembly metadata. In this file we added a couple of lines at the bottom:

```csharp


[assembly: XmlnsDefinition("https://avaloniademos.com/xaml", "Dependency2Proj")]
[assembly: XmlnsDefinition("https://avaloniademos.com/xaml", "Dependency2Proj.SubFolder")]
```

These two lines combine the two namespaces of the assembly: `Dependency2Proj` and `Dependency2Proj.SubFolder` into the same URL: "https://avaloniademos.com/xaml". As was mentioned above, it does not matter at all if that URL exists or of the computer is online. It is good if your URL would carry some meaning corresponding to the projects that contain this functionality.

Now the XAML prefix `dep2` by which we refer to both `GreenButton` and `GrayButton` is defined by referring to that URL:

```markup
xmlns:dep2="https://avaloniademos.com/xaml"
```

There is an important extra Avalonia feature in comparison to WPF. In WPF one can refer by URL only to the functionality that is not located in the same project as the XAML file that wants to refer to it, while in Avalonia, there is no such restriction - e.g. if we has a XAML file in the same Dependency2Proj project, we still could put the line 

```markup
xmlns:dep2="https://avaloniademos.com/xaml"
```

at its top element and refer to the our `GreenButton` and `GrayButton` defined within the same project by `dep2:` prefix.

### Accessing C\# Properties in XAML



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





