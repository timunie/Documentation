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

### Accessing C\# Composite Properties in XAML

We already mentioned above, that C\# built-in properties can be accessed as XML attributes of the corresponding element, e.g:

```markup
<my_namespace:MyClass Prop1="Hello World"
                      Prop2="123"/>
```

`Prop1` and `Prop2` are simple C\# properties defined on class `MyClass` which can be found in the C\# namespace referred to by `my_namespace` prefix of that XAML file. `Prop1` is likely of `string` type, while `Prop2` can be either of any numeric type or a string \(the XAML will automatically convert string "123" to the correct type\).

What will happen, however, if the property itself is of some complex type that contains several properties of its own.

C\# Solution [NP.Demos.AccessPropertiesInXamlSample.sln](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.AccessPropertiesInXamlSample) shows how to create such property in XAML.

There is a class `Person` defined within the project:

```csharp
namespace NP.Demos.AccessPropertiesInXamlSample
{
    public class Person
    {
        public int AgeInYears { get; set; }
        public string? FirstName { get; set; }
        public string? LastName { get; set; }

        public override string ToString()
        {
            return $"Person: {FirstName} {LastName}, Age: {AgeInYears}";
        }
    }
}

```

We want to display it as the Window's content. Here is what we have within MainWindow.xaml file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:NP.Demos.AccessPropertiesInXamlSample"
        x:Class="NP.Demos.AccessPropertiesInXamlSample.MainWindow"
        Width="300"
        Height="200"
        HorizontalContentAlignment="Center"
        VerticalContentAlignment="Center"
        Title="AccessPropertiesInXamlSample">
  <Window.Content>
    <local:Person FirstName="Joe" 
                  LastName="Doe"
                  AgeInYears="25"/>
  </Window.Content>
</Window>
```

Note the way we assign `Content` property of the `Window` to a composite Type:

```markup
<Window ...>
  <Window.Content>
    <local:Person FirstName="Joe" 
                  LastName="Doe"
                  AgeInYears="25"/>
  </Window.Content>
</Window>
```

we use `Window.Content` property tag with a period separating the name of the class from the name of the property. 

Note that in the same way as we assign properties of composite types, we can also assign primitive type properties, e.g. we can set the Window's Width by the following code:

```markup
<Window ...>
  <Window.Width>
    <x:Double>300</x:Double>
  </Window.Width>
</Window>
```

instead of using XML attributes. Of course, such notations are much bulkier than XAML attribute notations are are rarely used for properties of primitive types.

**Note**: Because `Window.Content` is a special property marked by `ContentAttribte`, we did not have to add `<Window.Content>` at all and could have placed `<local:Person .../>` object straight under `<Window...>` tag. There is only one property per class that can be marked with the `ContentAttribute` so in many cases, we are forced to use the `<Class.Property` notations anyway.

### XAML Special Properties

There are several special properties marked by prefix "x:", provided of course that we have "x" namespace prefix defined at the top of the file as:

```markup
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
```

The most important of them are `x:Name` and `x:Key`.

`x:Name` is used for elements within the XAML tree in order to be able to easily find an element in C\# and also \(by some people\) in order provide some self documentation for XAML and in order to be able to easily identify the element within Avalonia Development Tool \(as will be shown later\).

We already showed how to find an x:Name'd element in C\# code in [Introduction to Avalonia](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/#creating-and-running-a-simple-avalonia-application-under-visual-studio): one can use the `FindControl(...)` method e.g. for a button defined in XAML and x:Named `"CloseWindowButton"` we can use the following method in the code-behind to find it:

```csharp
var button = this.FindControl<Button>("CloseWindowButton");
```

`x:Key` is used for finding the Avalonia XAML resources and we are going to explain it in the section dedicated to them.

### Very Brief Introduction to Markup Extensions

Markup up extensions are some C\# classes that can significantly simplify XAML. They are used for setting some XAML properties using one liner notations that have curly brackets \('{' and '}'\) in them. There are some very important built-in Avalonia markup extensions - the most important are the following:

* StaticResource 
* DynamicResource
* x:Static
* Binding

We are going to provide examples for all of them except for the Binding \(which will be explained in a future chapter where the corresponding Avalonia Binding concept is elucidated\).

One can also create custom markup extensions, but this is rarely used and will not be touched upon in this guide. We shall explain it in one of the future guides.

### Avalonia XAML Resources

XAML resources is one of the most important methods of re-using XAML code and of placing some generic XAML code in generic Visual projects to be used in multiple applications.

#### StaticResource vs DynamicResource Sample

This sample shows the main difference between static and dynamic resources: static resource target value will not update when the resource itself is updated, while dynamic - will.

The sample is located under [NP.Demos.StaticVsDynamicXamlResourcesSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.StaticVsDynamicXamlResourcesSample) solution.

Open the solution and run it, here is what you will see:

![](../.gitbook/assets/image%20%2817%29.png)

Pressing "Change Status Color" button will result in the third rectangle switching its color to red:

![](../.gitbook/assets/image%20%2835%29.png)

Here is the MainWindow.xaml file for the sample:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.StaticVsDynamicXamlResourcesSample.MainWindow"
        Title="NP.Demos.StaticVsDynamicXamlResourcesSample"
        Width="300"
        Height="200">
  <Window.Resources>
    <ResourceDictionary>
      <!--We set the XAML resource-->
      <SolidColorBrush x:Key="StatusBrush" 
                       Color="Green"/>
    </ResourceDictionary>
  </Window.Resources>
  <Grid>
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="ElementsPanel" 
                Orientation="Vertical">
      <!--Refer to xaml resource using StaticResource Markup Expression -->
      <Border x:Name="Border1"
              Background="{StaticResource StatusBrush}"
              Height="30"
              Width="80"
              Margin="0,5"/>

      <!--Refer to xaml resource using StaticResource (without markup expression) -->
      <Border x:Name="Border2"
              Height="30"
              Width="80"
              Margin="0,5">
        <Border.Background>
          <StaticResource ResourceKey="StatusBrush"/>
        </Border.Background>
      </Border>

      <!--Refer to xaml resource using DynamicResource Markup Expression -->
      <Border x:Name="StatusChangingBorder"
              Background="{DynamicResource StatusBrush}"
              Height="30"
              Width="80"
              Margin="0,5"/>
    </StackPanel>
    <Button x:Name="ChangeStatusButton"
            Grid.Row="1"
            Width="160"
            HorizontalAlignment="Right"
            HorizontalContentAlignment="Center"
            Content="Change Status Color"
            Margin="10"/>
  </Grid>
</Window>

```

We define the XAML resource within the same MainWindow.xaml file as a resource of the window:

```markup
<Window.Resources>
  <ResourceDictionary>
    <!--We set the XAML resource-->
    <SolidColorBrush x:Key="StatusBrush" 
                     Color="Green"/>
  </ResourceDictionary>
</Window.Resources>
```

`x:Key` of the XAML resource can be used by `StaticResource` and `DynamicResource` to refer to the particular resource.

We then use `StaticResource` to set the background of two first borders and `DynamicResource` to the third border within a vertical border stack.

For the first border within the stack we use `StaticResource` markup extension:

```markup
<!--Refer to xaml resource using StaticResource Markup Expression -->
<Border x:Name="Border1"
      Background="{StaticResource StatusBrush}"
      Height="30"
      Width="80"
      Margin="0,5"/>
```

For the second border we use `StaticResource` class, without markup extension \(and you can see that the corresponding XAML is considerably more verbose\):

```markup
      <!--Refer to xaml resource using StaticResource (without markup expression) -->
      <Border x:Name="Border2"
              Height="30"
              Width="80"
              Margin="0,5">
        <Border.Background>
          <StaticResource ResourceKey="StatusBrush"/>
        </Border.Background>
      </Border>
```

Finally, the third border uses `DynamicResource` markup extension:

```markup
      <!--Refer to xaml resource using DynamicResource Markup Expression -->
      <Border x:Name="StatusChangingBorder"
              Background="{DynamicResource StatusBrush}"
              Height="30"
              Width="80"
              Margin="0,5"/>
```

Button "StatusChangingBorder" is hooked within MainWindow.xaml.cs file to change the "StatusBrush" Resource from "Green" to "Red":

```csharp
public MainWindow()
{
    InitializeComponent();

    Button button = 
            this.FindControl<Button>("ChangeStatusButton");

    button.Click += Button_Click;
}

private void Button_Click(object? sender, Avalonia.Interactivity.RoutedEventArgs e)
{
    // getting a Window resource by its name
    var statusBrush = this.FindResource("StatusBrush");
    
    // setting the window resource to a new value
    this.Resources["StatusBrush"] = 
                     new SolidColorBrush(Colors.Red);
}
```

Even though the resource is the same for all three border, only last border's background changes - the one that uses `DynamicResource`.

Other important differences between the static and dynamic resource are the following:

* `DynamicResource` can refer to a XAML resource defined in XAML below the `DynamicResource` expression, while `StaticResource` should refer to a resource above it. 
* `StaticResource` can be used to assign simple C\# properties on various objects used in XAML, while the target of a `DynamicResource` statement should always be a special Avalonia Property on AvaloniaObject \(special properties will be explained later\) 
* Since `DynamicResource` is more powerful \(provides change notification\) it takes considerably more memory resources than `StaticResource`. Because of that when you do not need the change notification \(the property stays the same for the duration of the program\) you should always use `StaticResource`. `DynamicResources` are very useful when you want to dynamically change the themes or the colors of your application, e.g. allow the user to switch the theme or to change the colors depending on the time of the day.

#### Referring to XAML Resources Defined in Different XAML Files and Projects Sample

In this sample we show how to refer to XAML resources located in a different file within the same or different project.

The sample is located under [NP.Demos.XamlResourcesInMultipleProjects](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.AccessPropertiesInXamlSample) Visual Studio solution. After running the sample you will see 3 rectangles of different colors - red, green and blue:

![](../.gitbook/assets/image%20%2832%29.png)

The solution consists of two projects - the main project NP.Demos.XamlResourcesInMultipleProjects and another project which the main project depends on - Dependency1Proj:

![](../.gitbook/assets/image%20%2833%29.png)

RedBrush resource is defined within Themes/BrushResources.axaml file under Dependency1Proj:

```markup
<ResourceDictionary xmlns="https://github.com/avaloniaui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <!-- Add Resources Here -->
  <SolidColorBrush x:Key="GreenBrush"
                   Color="Green"/>
</ResourceDictionary>
```

Note that the BrushResources.axaml file has "Avalonia XAML" build action \(as any Avalonia XAML resource file should\):

![](../.gitbook/assets/image%20%2834%29.png)

Such files are created by choosing "Resource Dictionary \(Avalonia\)" template for Visual Studio new item creation:

![](../.gitbook/assets/image%20%2836%29.png)

GreenBrush Avalonia Resource is defined within Themes/LocalBrushResources.axaml file \(this file is located in the main project\):

```markup
<ResourceDictionary xmlns="https://github.com/avaloniaui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <!-- Add Resources Here -->
  <SolidColorBrush x:Key="GreenBrush"
                   Color="Green"/>
</ResourceDictionary>
```

 Here is the content of MainWindow.axaml file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.XamlResourcesInMultipleProjects.MainWindow"
        Title="NP.Demos.XamlResourcesInMultipleProjects"
        Width="100"
        Height="180">
  <Window.Resources>
    <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceInclude Source="avares://Dependency1Proj/Themes/BrushResources.axaml"/>
        <ResourceInclude Source="/Themes/LocalBrushResources.axaml"/>
      </ResourceDictionary.MergedDictionaries>
      <!-- BlueBrush is defined locally -->
      <SolidColorBrush x:Key="BlueBrush"
                       Color="Blue"/>
    </ResourceDictionary>
  </Window.Resources>
  <StackPanel HorizontalAlignment="Center"
              VerticalAlignment="Center">
    <Border x:Name="RedBorder"
            Width="70"
            Height="30"
            Background="{StaticResource RedBrush}"
            Margin="5"/>
    <Border x:Name="GreenBorder"
            Width="70"
            Height="30"
            Background="{StaticResource GreenBrush}"
            Margin="5"/>
    <Border x:Name="BlueBorder"
            Width="70"
            Height="30"
            Background="{StaticResource BlueBrush}"
            Margin="5"/>
  </StackPanel>
</Window>
```

We have 3 borders stacked vertically - first border's background is getting its value from RedBrush resource, second border's - from GreenBrush and third border from BlueBrush. 

Take a look at the Resources section of the window at the top of the file:

```markup
<Window.Resources>
  <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceInclude Source="avares://Dependency1Proj/Themes/BrushResources.axaml"/>
        <ResourceInclude Source="/Themes/LocalBrushResources.axaml"/>
      </ResourceDictionary.MergedDictionaries>

      <!-- BlueBrush is defined locally -->
      <SolidColorBrush x:Key="BlueBrush"
                       Color="Blue"/>
    </ResourceDictionary>
  </Window.Resources>
```

`<ResourceInclude .../>` tags within `<ResourceDictionary.MergedDictionary>` tag means that we are merging the Resource Dictionaries defined externally to the current dictionary - they way we get all their key-value pairs. Those who know WPF can notice the difference - in WPF we user `<ResourceDictionary Source="..."/>` tag and not `<ResourceInclude Source="..."/>`. Also note that for a dependent project we do not separate the assembly from the rest of the URL and we are not using the cryptic "Component/" prefix for the URL. These are purely notational \(not conceptual\) differences, but still needs to be remembered.

Note the Avalonia XAML urls for the merged files:

* "avares://Dependency1Proj/Themes/BrushResources.axaml" - the url of the Avalonia XAML Resource file defined in a different project should start with the magic work "avares://" followed by the assembly name, followed by the path to the file: "avares://&lt;assembly-name&gt;/&lt;path-to-the-avalonia-resource\_file&gt;.
* "/Themes/LocalBrushResources.axaml" - the url of the Avalonia XAML Resource file defined in the same project in which it is used, should only consist of a forward slash followed by the path to avalonia resource file from the root of the current project.

At the end of the resource section, we define the BlueBrush resource - local to MainWindow.axaml file.

### x:Static Markup Extension

`x:Static` markup extension allows to refer to static properties defined in the same project or in some dependent projects. The sample code is located under [NP.Demos.XStaticMarkupExtensionSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.XStaticMarkupExtensionSample) solution. It contains two projects - the NP.Demos.XStaticMarkupExtensionSample \(main project\) and the dependency project Dependency1Proj. Main project contains class `LocalProjectStaticBrushes` while the dependency project contains `DependencyProjectStaticBrushes`

![](../.gitbook/assets/image%20%2838%29.png)

The contents of both C\# files are very simple - each defines and sets value for a single static property. Here is the content of `LocalProjectStaticBrushes` class:

```csharp
public class LocalProjectStaticBrushes
{
    public static Brush GreenBrush { get; set; } = 
        new SolidColorBrush(Colors.Green);
}
```

Here is the content of `DependencyProjectStaticBrushes` class:

```csharp
public class DependencyProjectStaticBrushes
{
    public static Brush RedBrush { get; set; } =
        new SolidColorBrush(Colors.Red);
}
```

Running the project will create a window with two rectangle red and green:

![](../.gitbook/assets/image%20%2839%29.png)

Here are the relevant parts of MainWindow.axaml file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:dep1="clr-namespace:Dependency1Proj;assembly=Dependency1Proj"
        xmlns:local="clr-namespace:NP.Demos.XStaticMarkupExtensionSample"
        ...>
  <StackPanel HorizontalAlignment="Center"
              VerticalAlignment="Center">
    <Border Width="70"
            Height="30"
            Background="{x:Static dep1:DependencyProjectStaticBrushes.RedBrush}"
            Margin="5" />
    <Border Width="70"
        Height="30"
        Background="{x:Static local:LocalProjectStaticBrushes.GreenBrush}"
        Margin="5" />
  </StackPanel>
</Window>
```

There are two important namespace defined at the Window tag level: 

```markup
xmlns:dep1="clr-namespace:Dependency1Proj;assembly=Dependency1Proj"
xmlns:local="clr-namespace:NP.Demos.XStaticMarkupExtensionSample"
```

"dep1" corresponds to the dependency project and "local" corresponds to the project local to the MainWindow.xaml file \(the main project\). 

Using these namespace prefixes and `x:Static` markup extension we can set the `Background` properties on the two borders:

```markup
Background="{x:Static dep1:DependencyProjectStaticBrushes.RedBrush}"
```

and 

```markup
Background="{x:Static local:LocalProjectStaticBrushes.GreenBrush}"
```

### Referencing Assets in XAML

In Avalonia Lingo - Assets are usually binary image \(e.g. png or jpg\) files. In this section we shall show how to refer to such files from `Image` controls within XAML.

The sample's code is located under [NP.Demos.ReferringToAssetsInXaml](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.ReferringToAssetsInXaml) solution. Here is the solution's code:

![](../.gitbook/assets/image%20%2818%29.png)

We have Themes/avalonia-32.png file under the dependent project Dependency1Proj and Themes/LinuxIcon.jpg file under the main project.

Note that the Build Action for the asset files should be "AvaloniaResource" \(unlike for XAML resource files where as we saw it was set to "Avalonia XAML"\):

![](../.gitbook/assets/image%20%2831%29.png)

Build and run the sample, here is what you'll see:

![](../.gitbook/assets/image%20%2837%29.png)

There are 4 vertically stacked images - here is the corresponding code:

```markup
<Image Source="/Assets/LinuxIcon.jpg" 
        Width="50"
        Height="50"
        Margin="5"/>
<Image Source="avares://Dependency1Proj/Assets/avalonia-32.png"
        Width="50"
        Height="50"
        Margin="5"/>
<Image x:Name="LinuxIconImage2"
        Width="50"
        Height="50"
        Margin="5"/>
<Image x:Name="AvaloniaIconImage2"
        Width="50"
        Height="50"/>
```

For the first two images - the Source is set in XAML, for the last - in C\# code behind. 

Note that the image defined as an asset local to the same project which contains our MainWindow.axaml file that uses it can use a simplified version of the source URL:

```markup
Source="/Assets/LinuxIcon.jpg" 
```

While the image located in a different project should be using a full version of the URL prepended with avares://

```markup
Source="avares://Dependency1Proj/Assets/avalonia-32.png"
```

Note, that just the same as in case of the XAML resource dictionary files and differently from WPF, the assembly name \("Dependency1Proj" - in our case\) is a part of the URL and there is no Component prefix.

The `Source` property of the last two images is being set within MainWindow.axaml.cs code-behind file. Here is the relevant code:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    ...

    // get the asset loader from Avalonia container
    var assetLoader = AvaloniaLocator.Current.GetService<IAssetLoader>();

    // get the Image control from XAML
    Image linuxIconImage2 = this.FindControl<Image>("LinuxIconImage2");

    // set the image Source using assetLoader
    linuxIconImage2.Source = 
        new Bitmap
        (
            assetLoader.Open(
                new Uri("avares://NP.Demos.ReferringToAssetsInXaml/Assets/LinuxIcon.jpg")));

    // get the Image control from XAML
    Image avaloniaIconImage2 = this.FindControl<Image>("AvaloniaIconImage2");

    // set the image Source using assetLoader
    avaloniaIconImage2.Source =
        new Bitmap
        (
            assetLoader.Open(
                new Uri("avares://Dependency1Proj/Assets/avalonia-32.png")));
}
```

Note that even for the local file "LinuxIcon.jpg" \(file defined in the same project as the MainWindow.xaml.cs file that uses it\), we need to provide the full URL with "avares://&lt;assembly-name&gt;/" prefix.

### Non-Visual XAML Code

The last sample will demonstrate that potentially one can use XAML even for a completely non-visual code. The sample is located under [NP.Demos.NonVisualXamlSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.XamlSamples/NP.Demos.NonVisualXamlSample) solution. Unlike the previous samples - it is a console application referencing only one \(not three\) Avalonia nuget packages:

![](../.gitbook/assets/image%20%2824%29.png)

The main program is located under Program.cs file and is very simple:

```csharp
        public static void Main(string[] args)
        {
            Course course = new Course();
        }
```

You can put a breakpoint after the line and investigate the content of the course object:

![](../.gitbook/assets/image%20%2830%29.png)

Take a look at Course.axaml/Course.axaml.cs files. Here is the content of Course.axaml file:

```markup
<x:Object xmlns="https://github.com/avaloniaui"
          xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
          xmlns:local="clr-namespace:NP.Demos.NonVisualXamlSample"
          x:Class="NP.Demos.NonVisualXamlSample.Course"
          NumberStudents="5">
    <local:Person FirstName="Joe" LastName="Doe" Age="100" />
</x:Object>
```

The top tag of type `Course` contains a single object of type `Person`. The `NumberStudents` property of the top object is set to 5, while the `Person's` properties are set `FirstName="Joe"`, `LastName="Doe"` and `Age="100"`.

Course.axaml.cs file defines the properties for the `Course` class:

```csharp
public partial class Course
{
    public Course()
    {
        AvaloniaXamlLoader.Load(this);
    }

    public int NumberStudents { get; set; }

    [Content]
    public Person? Teacher { get; set; }
}
```

Note that its constructor also defines the method for loading the XAML file and that the class is marked as "partial" \(the other part is generated from XAML\). Also note that `Teacher` property has `ContentAttribute` - this is why we do not need to use `<local:Course.Teacher>` tag to place the `Person` object into.

Finally, here is the code for `Person` class:

```csharp
public class Person
{
    public string? FirstName { get; set; }

    public string? LastName { get; set; }

    public double Age { get; set; }
}
```

### XAML Topics not Covered in this Chapter

Avalonia Attached Properties, Bindings, Styles, Control and Data Templates will be covered for XAML in the same chapters in which we cover the corresponding concepts.

## Visual Trees

Avalonia \(and WPF\) **basic** building blocks \(the primitives\) consist of:

1. **Primitive elements** - the very basic elements like `TextBlock`, `Border`, `Path`, `Image`, `Viewbox`, etc. that cannot be decomposed into sub-elements within Avalonia universe.
2. **Panels** - elements responsible for arranging other elements within them.

The rest of the controls \(more complex controls including such basic controls as e.g., `Button`, `ComboBox`, `Menu`, etc.\) and complex views are built by putting the various primitives together by placing them within other primitives or panels. In Avalonia, the primitives usually inherit from `Control` class, while the more complex controls inherit from `TemplatedControl` class, while in WPF, the primitives inherit from `Visual` and the more complex controls inherit from `Control` \(in WPF, `Control` has the `Template` property and related infrastructure, while in Avalonia, it is `TemplatedControl` that has them\). You can read more about Avalonia primitives in [Avalonia Primitives ](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/building-avalonia-applications/avalonia-building-blocks-explained-with-samples#avalonia-primitives)section of the previous chapter.

The composition of Avalonia \(and WPF\) visual object can be hierarchical: we create some simpler objects out of the primitives and then create more complex objects out of those simpler objects \(and perhaps also the primitives\), etc. This principle of hierarchical composition is one of the core ways or re-using the visual components.

The figure below shows that a simple button might consist of several primitive elements: e.g., it might consist of a `Grid` panel that has a `TextBlock` object for the text of the button and an `Image` object for the button's icon. This containment structure of objects clearly defines a simple tree - the Visual Tree.

Here is the figure of a very simple button described above:

![](https://www.codeproject.com/KB/Articles/5311995/ButtonContainmentStructure_small.png)

And here is the figure of the button's visual tree:

![](https://www.codeproject.com/KB/Articles/5311995/ButtonVisualTree.png)

Of course, the real button's visual trees might be more complex and also include borders for button's border and shade and an overlay panel or panels which change opacity or color once the mouse is over the button to indicate that this button is active on mouse click and many other things, but for the sake of explaining the concept of visual tree, the button described above is ok.

Now start [NP.Demos.VisualTreeSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.VisualTreeSample) solution. The only files changed from the default content in this solution are MainWindow.axaml \(.axaml files are very much the same as .xaml files only used by Avalonia in order for them to coexist with WPF .xaml files\) and MainWindow.axaml.cs. You can find more about the files in the AvaloniaUI application project at by reading ["Creating and Running a Simple Avalonia Application under Visual Studio"](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/#creating-and-running-a-simple-avalonia-application-under-visual-studio) guide.

Here is the content of MainWindow.xaml:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.VisualAndLogicalTreeSample.MainWindow"
        Title="NP.Demos.VisualAndLogicalTreeSample"
        Width="300"
        Height="200">
  <Button x:Name="SimpleButton"
          Content="Click Me" 
          HorizontalAlignment="Center"
          VerticalAlignment="Center"/>
</Window>  
```

Take a brief look also at App.axaml file. You will see a reference to the `FluentTheme`:

```markup
<Application.Styles>
    <FluentTheme Mode="Light"/>
</Application.Styles>
```

Themes define the look and behavior for all major controls, including, of course the buttons. They do it by using Styles and Templates, how exactly - will be explained later in these series of the guides. It is important to understand, that our `Button`'s Visual Tree is defined by the `Button`'s `ControlTemplate` located within the `Button`'s style which is in turn located within the `FluentTheme`.

Here is what you see when you run the project:

![](https://www.codeproject.com/KB/Articles/5311995/VisualAndLogicalTreeSample.png)

Click on the window for it to get the mouse focus and press the **F12** key. The Avalonia Tool window will open:

![](https://www.codeproject.com/KB/Articles/5311995/ToolWindow.png)

The tool window is analogous to [WPF snoop](https://github.com/snoopwpf/snoopwpf) \(even though it is still less powerful than WPF snoop in some aspects\). It gives you the ability to investigate any property or event on any element within the visual or logical trees.

In Avalonia, logical tree \(an explanation of what it is will be provided shortly\) plays a bigger role than in WPF, so by default, the tool shows the Logical Tree, and in order to switch to the Visual Tree, you need to click "**Visual Tree**" tab \(highlighted in the image above by the read ellipse\).

Once we switched the Tool to display the Visual Tree, press Control and Shift keys together and place the mouse over the button's text. The Visual Tree on the left side of the tool will expand to the element containing the `Button`'s text and the property pane in the middle of the tool will show the properties of the currently selected element of the Visual Tree \(which in our case will be the `Button`'s `TextBlock` element:

![](https://www.codeproject.com/KB/Articles/5311995/ToolDisplayingVisualTree.png)

The Visual Tree is actually shown for the whole window \(and part of it corresponding to the currently selected element is expanded\).

You can see that the Visual Tree for the `Button` from the `FluentTheme` is actually even simpler than the one we considered above: it consists only of the following three elements - `Button` \(the root element\), then `ContentPresenter` and then `TextBlock` element:

![](https://www.codeproject.com/KB/Articles/5311995/FluentButtonVisualTree.png)

You can select a different element, say the `Button` - which is the grand parent of the `TextBlock` to see the `button`'s properties in the middle pane of the Tool. If you are looking for a particular property, e.g., `DataContext`, you can type part of its name on top of the property table, e.g., "`context`" and it will filter the properties to those whose names contain the word "`context`":

![](https://www.codeproject.com/KB/Articles/5311995/ToolPropertyFiltering.png)

More about the Tool will be said in the next section.

Example of C\# functionality for obtaining the Visual Tree nodes in C\# code is located within MainWindow.xaml.cs file within `OnButtonClick` method:

```csharp
private void OnButtonClick(object? sender, Avalonia.Interactivity.RoutedEventArgs e)
{
    IVisual parent = _button.GetVisualParent();

    var visualAncestors = _button.GetVisualAncestors().ToList();

    var visualChildren = _button.GetVisualChildren().ToList();

    var visualDescendants = _button.GetVisualDescendants().ToList();
}  
```

This method is attached to handle the button's click event:

```csharp
_button = this.FindControl<Button>("SimpleButton");

_button.Click += OnButtonClick; 
```

Note that in order to make the Visual Tree extension methods available, we had to add `using Avalonia.VisualTree;` namespace reference at the top of MainWindow.axaml.cs file.

Put a breakpoint at the very end of the method, and click the button. You can investigate the content of the variables within `OnButtonClick()` method within a `Watch` window:

![](https://www.codeproject.com/KB/Articles/5311995/VisualTreeVariables.png)

You can see that the results are consistent with the Visual Tree observed in the Tool:

![](https://www.codeproject.com/KB/Articles/5311995/ButtonVisualTreeHighlighted.png)

Indeed,

1. our `Button`'s parent is a `ContentPresenter`,
2. our `Button` has four ancestors: `ContentPresenter`, `VisualLayoutManager`, `Panel` and `Window`
3. our `Button` has only one child - a `ContentPresenter`,
4. and our `Button` has two descendants: a `ContentPresenter` and a `TextBlock`.

## Avalonia Tool

Once we mentioned the Avalonia Tool in the previous section, let us give a bit more information about it here.

The beauty of the tool is that it is also written in Avalonia and therefore it is multiplatform. It will also show on MacOS and Linux if you want to check the trees and the properties on those platforms - all you need to do is to click on the window for which you want the tool to work and press the **F12** key.

The tool shows only the information corresponding to a single window, so if you are using multiple windows whose trees and properties you want to investigate, you'll have to use multiple tool windows.

The tool will not show up for a configuration that does not have `DEBUG` preprosessor variable set, for example, the default release configuration. In fact, the following lines from the `MainWindow` constructor \(located in _MainWindow.axaml.cs_ file\) create the ability to start the tool:

```csharp
#if DEBUG
            this.AttachDevTools();
#endif  
```

Also if you do not need the tool, once you remove the call to `this.AttachDevTool()`, you can also remove `Avalonia.Diagnostics` package from your references.

## Logical Trees

Logical Tree is a subset of the Visual Tree - it is sparser than the Visual Tree - has fewer elements in it. It closely follows the XAML code, but it does not expand any control templates \(what they are will be explained in a future article\). When a `ContentControl` is displayed, it goes straight from the `ContentControl` to the element that represents its `Content` \(omitting everything in between\). When an `ItemsControl` is displayed, it goes straight from the `ItemsControl` element to the elements that represent the contents of its items, also omitting everything in between. More detailed explanations of this will be given in the chapter dedicated to DataTemplates and View Models.

The code is located under [NP.Demos.LogicalTreeSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.LogicalTreeSample) solution. This is what you are going to see once you run it:

![](https://www.codeproject.com/KB/Articles/5311995/LogicalTreeSample.png)

Here is the XAML code producing this layout from MainWindow.axaml file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.LogicalTreeSample.MainWindow"
        Title="NP.Demos.LogicalTreeSample"
        Width="300"
        Height="200">

  <Grid RowDefinitions="*, *">
    <Button x:Name="ClickMeButton" 
            Content="Click Me"
            HorizontalAlignment="Center"
            VerticalAlignment="Center"/>

    <ItemsControl Grid.Row="1"
                  HorizontalAlignment="Center"
                  VerticalAlignment="Center">
      <Button x:Name="Item1Button" 
              Content="Item1 Button"/>
      <Button x:Name="Item2Button"
              Content="Item2 Button"/>
    </ItemsControl>
  </Grid>
</Window>  
```

We see that the window's content is represented by the `Grid` panel with two rows. Top row contains the `Button` "Click Me" and the bottom row contains an `ItemsControl` with two buttons: "Item1 Button" and "Item2 Button". The XAML names of the buttons are the same as what is written in them only without spaces: "`ClickMeButton`", "`Item1Button`" and "`Item2Button`".

Click on the sample's window, then press **F12** to start the Tool and expand the Logical Tree within the tool - here is what you'll see:

![](https://www.codeproject.com/KB/Articles/5311995/LogicalTreeTool.png)

You can see that only the elements that correspond to XAML tags of MainWindow.axaml file are present in the visual tree plus the `TextBoxes` within the `Buttons` \(since the button is a `ContentControl` - the `TextBoxes` are elements that represent its content\). Many nodes that would be present in the Visual Tree are missing here - you will not find here Visual Tree elements that were created due to the expansion of the control templates - there are no Window's borders, Panels, `VisualLayoutManager`, etc., we skip from Window straight to the `Grid` because `Grid` element is part of MainWindow.axaml file. Likewise, we skip from the `Button` straight to the `TextBlock` omitting the `ContentPresenter` since it comes from the `Button's` Template expansion.

Now take a look at `OnButtonClick` method within MainWindow.axaml.cs file

```csharp
private void OnButtonClick(object? sender, RoutedEventArgs e)
{
    ItemsControl itemsControl = 
                    this.FindControl<ItemsControl>("TheItemsControl");

    var logicalParent = itemsControl.GetLogicalParent();
    var logicalAncestors = itemsControl.GetLogicalAncestors().ToList();
    var logicalChildren = itemsControl.GetLogicalChildren().ToList();
    var logicalDescendants = itemsControl.GetLogicalDescendants().ToList();
}  
```

We are getting the logical parent, ancestors, children and descendants of the `ItemsControl` element. This method is set to be a `Click` event handler of the `ClickMeButton`:

```csharp
Button clickMeButton = this.FindControl<Button>("ClickMeButton");
clickMeButton.Click += OnButtonClick;  
```

Note that in order to get those extension methods, we had to add `using Avalonia.LogicalTree` namespace at the top of MainWindow.xaml.cs file.

Put a breakpoint, at the very end of the method, run the application and press "ClickMeButton". Check the variables in the **Watch** window:

![](https://www.codeproject.com/KB/Articles/5311995/LogicalTreeNavigation.png)

Which corresponds exactly to what we saw in the Tool.

## Avalonia Special Properties

### Attached Properties

Attached Properties is an extremely important and useful concept to understand. It was originally introduced by WPF and from there, it got straight to Avalonia, though in a better and expanded version.

In order to explain what Attached Property is, let us start by remembering what a simple read/write property is in C\#. Essentially a property of type `T` defined in a class `MyClass` can be represented by two methods - a getter and a setter methods:

```csharp
public class MyClass<Т>
{
  T Getter();
  void Setter(T value);
}
```

Usually, such property is implemented by a backing field of type `T` defined within the same class:

```csharp
public class MyClass<T>  
{
  // the backing field
  T _val;

  T Getter() => _val;
  void Setter(T value) => _val = value;
} 
```

During the work on WPF, the WPF architects were facing an interesting problem. Every visual object had to define hundreds if not thousands of properties, most of which at each time would have default values. Defining a backing field for each property within each object would lead to a huge consumption of memory especially unnecessary because ~90% of those properties at each time would have a default value in them.

So, to get around this problem, they came up with the the so called Attached Properties. Instead of storing the property value in a backing field inside the object, the Attached Property stores the values in a kind of a `static hashtable` or `Dictionary` \(or `Map`\) where the values are indexed by the various objects that have those properties. Only the objects with non-default property values are within the `hashtable`, if the entry for an object is not in the `hashtable`, it is assumed that the object's property has the default value. The static hash table of the Attached Property can be defined practically in any class - often, it is defined in a different class than the one that uses its values. So very **roughly** \(and approximately\) speaking - the implementation of an attached property `MyAttachedProperty` of type say `double` on class `MyClass` would be somewhat like:

```csharp
public class MyClass
{

}

public static class MyAttachedPropertyContainer
{
    // Attached Property's default value
    private static double MyAttachedPropertyDefaultValue = 5.0;

    // Attached Property's Dictionary
    private static Dictionary<MyClass, double> MyAttachedPropertyDictionary =
                                              new Dictionary<MyClass, double>();

    // property getter
    public static double GetMyAttachedProperty(this MyClass obj)
    {
        if (MyAttachedPropertyDictionary.TryGetValue(obj, out double value)
        {
            return value;
        }
        else // there is no entry in the Dictionary for the object
        {
            return MyAttachedPropertyDefaultValue; // return default value
        }
    }

    // property setter
    public static SetMyAttachedProperty(this MyClass obj, double value)
    {
        if (value == MyAttachedPropertyDefaultValue)
        {
           // since the property value on this object 'obj' should become default,
           // we remove this object's entry from the Dictionary -
           // once it is not found in the Dictionary, - the default value will be returned
           MyAttachedPropertyDictionary.Remove(obj);
        }
        else
        {
            // we set the object 'to have' the passed property value
            // by setting the Dictionary cell corresponding to the object
            // to contain that value
            MyAttachedPropertyDictionary[obj] = value;
        }
    }
}
```

So instead of each object of type `MyClass` containing the value, the value is located in some `static Dictionary` indexed by the objects of the type `MyClass`. One can also specify some default value for the property \(in our case, it is 5.0\) so that only objects with non-default property value will require an entry in the `Dictionary`.

Such approach saves tons of memory at the expense of slightly slower getter and setter for the property.

Once the Attached Properties were tried, it was discovered that on top of memory saving, they give a lot of other benefits - e.g.:

* You can easily add some property change notification callbacks to them which will fire once the property changes on an object.
* You can define an attached property on a class without modifying the class itself. This is extremely important. A glaring example - is the usual button does not have a `CornerRadius` property. Assume that you have many buttons of different types in your application and suddenly, the users request that many of them should have some smooth border corners and besides different button should have different corner radiuses. You do not want to create a new derived type for the buttons and replace them everywhere and retest every one of them, but you are ok with slightly modifying the buttons' styles. You can create the Attached Property `TheCornerRadiusProperty`, bind the buttons' borders' `CornerRadius` property `TheCornerRadiusProperty` of the button and set this property to the required values within individual buttons' styles.
* Generalizing the previous item, the Attached Properties allow creating and attaching behaviors to the visual objects - behaviors are complex classes that allow modifying and augmenting the visual object's functionality without modifying the visual object's class. Behaviors are a bit too complex for this chapter and will be described in the future.

Of course, the very simple implementation shown above, did not take into consideration a lot of other issues like threading, callbacks, registration \(in order to know all the attached properties allowed on our class `MyClass`\) and so on. Besides, it is ugly to define the default value as a `static` variable by itself outside of the property as we did above. Because of these considerations, it makes sense to create a special type \(possibly with some generic arguments\) `AttachedProperty<...>` which will contain the `Dictionary`, the default value and a lot of other functionality required for the property to function. This is what WPF and Avalonia did.

Before we move on to the Attached Property sample, it is a good idea to download the Avalonia snippets available at [Avalonia Snippets](https://github.com/npolyak/NP.Avalonia.Visuals/tree/main/AvaloniaSnippets) and install them. Installation instructions can be found at the same URL.

The Attached Property sample is located under [NP.Demos.AttachedPropertySample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.AttachedPropertySample) solution. Try running it. Here is what you'll see:

![](https://www.codeproject.com/KB/Articles/5311995/AttachedPropSample.png)

The slider changes can change between values `0` and `10` and when you change the slider position, the `StrokeThickness` property of the rectangle changes correspondingly - the rectangle become thicker or thinner \(when slider position is at `0`, rectangle disappears completely\).

Take a look at the contents of AttachedProperties.cs file - the `RectangleStrokeThickness` Attached Property is defined there. This property was created by using _avap_ snippet \(its name stands for Avalonia Attached Property\):

```csharp
public static class AttachedProperties
{
    #region RectangleStrokeThickness Attached Avalonia Property

    // Attached Property Getter
    public static double GetRectangleStrokeThickness(AvaloniaObject obj)
    {
        return obj.GetValue(RectangleStrokeThicknessProperty);
    }

    // Attached Property Setter
    public static void SetRectangleStrokeThickness(AvaloniaObject obj, double value)
    {
        obj.SetValue(RectangleStrokeThicknessProperty, value);
    }

    // Static field that of AttachedProperty<double> type. This field contains the
    // Attached Properties' Dictionary, the default value and the rest of the required 
    // functionality
    public static readonly AttachedProperty<double> RectangleStrokeThicknessProperty =
        AvaloniaProperty.RegisterAttached<object, Control, double>
        (
            "RectangleStrokeThickness", // property name
            3.0 // property default value
        );

    #endregion RectangleStrokeThickness Attached Avalonia Property
}  
```

We can see that:

* `public static double GetRectangleStrokeThickness(AvaloniaObject obj)` is the getter \(similar to the one in discussion above\),
* `public static void SetRectangleStrokeThickness(AvaloniaObject obj, double value)` is the setter.
* `public static readonly AttachedProperty<double> RectangleStrokeThicknessProperty</double>` is the `static` field containing the `Dictionary` \(or object to value `hashtable`\) and the Attached Property's default value and all the rest of the needed functionality.

The amount of code needed to define an Attached Property looks formidable, but it was all created within a couple of seconds with the help of the _avap_ snippet. So if you plan to work with Avalonia - snippets are a must \(same as in WPF\). You can also see that my snippet puts each Attached Property within its own region, so that it can be collapsed and the code can be made more readable.

Now, take a look at the XAML code inside MainWindow.cs file:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:NP.Demos.AttachedPropertySample"
        x:Class="NP.Demos.AttachedPropertySample.MainWindow"
        Title="NP.Demos.AttachedPropertySample"
        local:AttachedProperties.RectangleStrokeThickness="7"
        Width="300"
        Height="300">
  <Grid RowDefinitions="*, Auto">
        <Rectangle Width="100"
                   Height="100"
                   Stroke="Green"
                   StrokeThickness="{Binding Path=(local:AttachedProperties.RectangleStrokeThickness), 
                                             RelativeSource={RelativeSource AncestorType=Window}}"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
    
      <Slider Minimum="0"
              Maximum="10"
              Grid.Row="1"
              Value="{Binding Path=(local:AttachedProperties.RectangleStrokeThickness), 
                              Mode=TwoWay, 
                              RelativeSource={RelativeSource AncestorType=Window}}"
              Margin="10,20"
              HorizontalAlignment="Center"
              VerticalAlignment="Center"
              Width="150"/>
  </Grid>
</Window>  
```

Note that in XAML, we are using Bindings - a very important concept that will be explained later in more detail.

We have a `Grid` panel with two rows - there is a `Rectangle` in the top row and a `Slider` control at the bottom. The `Slider` can change its value between `0` and `10`.

At almost the very top - there is the following line within the `Window` XAML tag:

```markup
xmlns:local="clr-namespace:NP.Demos.AttachedPropertySample"  
```

This line defines the local XAML namespace so that via that namespace, we can refer to our `RectangleStrokeThickness` Attached Property.

The next interesting line is:

```text
local:AttachedProperties.RectangleStrokeThickness="7"  
```

Here, we are setting the initial value of the `RectangleStrokeThickness` Attached Property on the window object to number `7`. Note how we specify the Attached Property: `<namespace-name>:<class-name>.<AttachedProperty-name>`.

The line

```markup
StrokeThickness="{Binding Path=(local:AttachedProperties.RectangleStrokeThickness), 
                          RelativeSource={RelativeSource AncestorType=Window}}"
```

under `Rectangle` tag binds the rectangle's property `StrokeThickness` to the Attached Property `RectangleStrokeThickness` on the window ancestor of the rectangle. Note the format of the Attached Property within the `Binding` - the Attached Property full name is within parentheses - this is a requirement both in Avalonia and WPF - without parentheses, the binding will not work and people can spend hours trying to figure out what is wrong.

`Slider`'s line:

```markup
Value="{Binding Path=(local:AttachedProperties.RectangleStrokeThickness), 
                Mode=TwoWay, 
                RelativeSource={RelativeSource AncestorType=Window}}"  
```

binds the `Value` property of the `Slider` to the `RectangleStrokeThickness` Attached Property of the window ancestor of the slider \(which of course is the same `Window` object as the window ancestor of the `Rectangle`\). This binding is a `TwoWay` binding - meaning changes to the `Slider`'s `Value` property will also change the `RectangleStrokeThickness` Attached Property value on the `Window`.

The principle of operation of this view is simple - changing the `Slider's` value by moving the `Slider's` so called thumb - will trigger the change of the `RectangleStrokeThickness` Attached Property on the Window \(via the Slider's binding\) and this, in turn, will trigger the change of the `StrokeThickness` property on the `Rectangle` \(via its binding\).

Of course, in this simple case, we could have connected the `Slider`'s `Value` to the `Rectangle`'s `StrokeThickness` property directly without involving the Attached Property on the Window, but then the example would not demonstrate how Attached Properties work \(and in many cases, e.g., when a needed property does not exist on a control Attached Properties are a must\).

Now try removing the line that sets initial value to 7 at the top:

```markup
local:AttachedProperties.RectangleStrokeThickness="7"  
```

and restart the application. You will see that the initial value of the `Rectangle's` `StrokeThickness` and the `Slider`'s `Value` became `3.0` and not `7.0`. This is because the default value for our Attached Property is `3.0` as was defined when the Attached Property was registered.

Now let us discuss the Attached Properties change notification.

Take a look at the file MainWindow.axaml.cs: here is the interesting code from that file:

```csharp
public partial class MainWindow : Window
{
    // to stop change notification dispose of this subscription token
    private IDisposable _changeNotificationSubscriptionToken;

    public MainWindow()
    {
        InitializeComponent();

        ...

        // subscribe
        _changeNotificationSubscriptionToken =
            AttachedProperties
                .RectangleStrokeThicknessProperty
                .Changed
                .Subscribe(OnRectangleStrokeThicknessChanged);
    }

    // this method is called when the Attached property changes
    private void OnRectangleStrokeThicknessChanged
    (
        AvaloniaPropertyChangedEventArgs<double> changeParams)
    {
        // if the object on which this attached property changes
        // is not this very window, do not do anything
        if (changeParams.Sender != this)
        {
            return;
        }

        // check the old and new values of the attached property. 
        double oldValue = changeParams.OldValue.Value;

        double newValue = changeParams.NewValue.Value;
    }  

    ...
}
```

At the top, we define the subscription token - it is `IDisposable` so if we want to stop reacting to the subscription change, we can call `_changeNotificationSubscriptionToken.Dispose()`.

Subscription to the Attached Property changes happens in the constructor:

```csharp
// subscribe
_changeNotificationSubscriptionToken =
    AttachedProperties
        .RectangleStrokeThicknessProperty
        .Changed
        .Subscribe(OnRectangleStrokeThicknessChanged);  
```

And the method `void OnRectangleStrokeThicknessChanged(...)` is called when the value changes. The method accepts a single argument of type `AvaloniaPropertyChangedEventArgs<double>` and this argument contains all required information:

1. The object on which the Attached Property changed provide by the property `Sender`
2. `OldValue` property carries information about the previous value.
3. `NewValue` property carries information about the current value.

You can put a debug breakpoint at the end of the method, start the application on the debugger and try to move the slider - you will stop on the breakpoint and will be able to investigate the current values.

Another, easier way of doing approximately the same \(without ability to terminate the subscription\) is by creating a `static` constructor within MainWindow.axaml.cs file and using the `AddClassHandler` extension method:

```csharp
static MainWindow()
{
    AttachedProperties
        .RectangleStrokeThicknessProperty
        .Changed
        .AddClassHandler<MainWindow>((x, e) => x.OnAttachedPropertyChanged(e));
}

private void OnAttachedPropertyChanged(AvaloniaPropertyChangedEventArgs e)
{
    double? oldValue = (double?) e.OldValue;

    double? newValue = (double?)e.NewValue;
}  
```

Note that here, you do not need to check that the sender is the same as the current object.

You can see that `OnAttachedPropertyChanged(...)` method has a slightly less type safe signature. Usually, this way is perfectly good and 99% of times, you'll be able to achieve what you need using `AddClassHandler(...)`.

As you probably noticed, Avalonia is using a powerful `IObservable` Reactive Extensions paradigm when it comes to the Attached Properties change notifications.

### Style Properties

WPF has a concept of Dependency Properties which are basically the same as Attached Properties, only they are defined within the same class that uses them and correspondingly their getters and setter are placed within the `class's` property of the same name. Note that with Dependency Properties, we still have the advantage of not wasting memory on defaults and adding callbacks easily, but we lose the advantage of adding a property without modifying the class.

I tried using locally defined Attached Properties in Avalonia, and I have not noticed anything wrong with them, but according to Avalonia documentation, it is better to use the so called Style Properties instead \(why - I am not sure at this point\).

We shall follow the documentation and run a sample showing how to use the so called Style Properties.

For the sample, open [NP.Demos.StylePropertySample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.StylePropertySample) solution.

The sample will run in exactly the same fashion as the previous one and the code is very similar, only instead of using `RectangleStrokeThickness` Attached Property defined in _AttachedProperties.cs_ file, we are using the Style Property of the same name defined within MainWindow.axaml.cs file. You can see that the getter and the setter of the `Style` Property are non-static and are considerably simpler:

```csharp
#region RectangleStrokeThickness Styled Avalonia Property
public double RectangleStrokeThickness
{
    // getter 
    get { return GetValue(RectangleStrokeThicknessProperty); }

    // setter
    set { SetValue(RectangleStrokeThicknessProperty, value); }
}

// the static field that contains the hashtable mapping the 
// object of type MainWindow into double and also containing the 
// information about the default value
public static readonly StyledProperty<double> RectangleStrokeThicknessProperty =
    AvaloniaProperty.Register<MainWindow, double>
    (
        nameof(RectangleStrokeThickness)
    );
#endregion RectangleStrokeThickness Styled Avalonia Property  
```

This `style` property was also created in seconds by using my other snippet - _avsp_ that stands for Avalonia Style Property. In XAML style properties are accessed in exactly the same way as the usual properties - since they have usual getter and setter.

### Direct Properties

Sometimes, one wants to use a simple C\# property backed by a field, and yet to be able to subscribe to its changes and to use the property as a target of some binding - **yes only Attached, Style and Direct properties can be used as targets of the Avalonia bindings**. The simple C\# properties can still be used as the **source** of the bindings, through, providing the change notification via firing the `PropertyChanged` event of `INotifyPropertyChanged` interface.

The direct property sample is located in [NP.Demos.DirectPropertySample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.DirectPropertySample) solution. The demo behaves in exactly the same way as the previous two demos, only we are using the Direct Property instead of `Style` or Attached Property.

Here is how the Direct Property is defined within the MainWindow.xaml.cs file:

```csharp
#region RectangleStrokeThickness Direct Avalonia Property
private double _RectangleStrokeThickness = default;

public static readonly DirectProperty<MainWindow, double> RectangleStrokeThicknessProperty =
    AvaloniaProperty.RegisterDirect<MainWindow, double>
    (
        nameof(RectangleStrokeThickness),
        o => o.RectangleStrokeThickness,
        (o, v) => o.RectangleStrokeThickness = v
    );

public double RectangleStrokeThickness
{
    get => _RectangleStrokeThickness;
    set
    {
        SetAndRaise(RectangleStrokeThicknessProperty, ref _RectangleStrokeThickness, value);
    }
}

#endregion RectangleStrokeThickness Direct Avalonia Property  
```

This Direct Property was created in seconds, by using _avdr_ snippet \(its name stands for Avalonia Direct\).

#### More Information about Attached, Style and Direct Properties

`AttachedProperty<...>`, `StyleProperty<...>` and `DirectProperty<...>` classes all derive from `AvaloniaProperty` class.

As was mentioned above, only Attached, Style and Direct Properties can be made a target of Avalonia UI bindings.

The Attached, Style and Direct Properties can only be set on classes that implement `AvaloniaObject` - which is a very basic class that all Avalonia visuals implement.

If you do not need the previous value of a changed variable \(`OldValue` in the samples we had above\), the best way to subscribe to the changes of Attached, Style and Direct properties would be to use the `AvaloniaObject.GetObservable(AvaloniaProperty property)` method.

To demonstrate using `GetObservable(...)` method we can modify our Attached Property sample as follows:

```csharp
public MainWindow()
{
...
_changeNotificationSubscriptionToken =
    this.GetObservable(AttachedProperties.RectangleStrokeThicknessProperty)
        .Subscribe(OnStrokeThicknessChanged);
}

private void OnStrokeThicknessChanged(double newValue)
{
...
}
```

You can see that `OldValue` is no longer available within the callback.

## Avalonia Bindings

### **What is a Binding in Avalonia UI and WPF and Why It Is Needed**

Binding is an extremely powerful concept that allows to bind two properties so that when one of them changes, the other will change also. Usually, binding works from the `source` property to the `target` property \(normal `OneWay` binding\), but there is also a `TwoWay` binding which makes sure that the two properties are in sync whichever initiates the change. There are two more binding modes: `OneWayToSource` and `OneTime` which are used considerably less often.

There are also less discussed, but just as important **collection bindings** where one collection would mimic another or the two collections would mimic each other.

Note that the target of the binding does not have to be exactly the same the binding's source, there can be conversion employed between the source and the target and vice versa as will be shown below.

Binding is the main concept behind the so called MVVM pattern \(which will be discussed in detail in one of the future guides\). The core idea of the MVVM pattern is that the complex Visual Objects are mimicking the properties and behaviors of very simple non-Visual objects - the so called View Models \(VMs\):

![](https://www.codeproject.com/KB/Articles/5311995/Mimicking.jpg)

Because of that, most of the business logic can be developed and tested on simple non-visual objects and then transmitted by the bindings to the very complex visual object which will automatically behave in the similar fashion.

### **Good Things about the Avalonia Bindings**

Avalonia bindings are considerably more powerful, less buggy and quirky and easier to use than WPF bindings - the reason is that they have been built much more recently by very bright people \(or person\) [Steven Kirk](https://github.com/grokys) who apparently loved the WPF bindings, knew about their quirks and limitations and moreover knew about more recent advances in software development theory and practice - [Reactive Extensions](http://reactivex.io/).

### **Avalonia Binding Concepts**

Avalonia Binding is a complex object with many capabilities some of the most important of which, I will discuss in this subsection.

Avalonia \(and WPF\) bindings are best explained by the figure below:

![](https://www.codeproject.com/KB/Articles/5311995/PropertyBinding.png)

Following parts of the Binding class are important:

1. **Binding Source Object** - The object through which one can obtain the path to the binding source property.
2. **Binding Target Object** - The object whose `AvaloniaProperty` \(Attached, Style or Direct\) property serves as a target for the binding. Target Object can only be of class derived from `AvaloniaObject` \(which means it can be any of the Avalonia visuals\). `AvaloniaObject` is analogous to the `DependencyObject` of WPF.
3. **Binding Path** - Path from the source object to the source property. Path consists of path links, each of which can be a regular \(C\#\) property or an Avalonia Property. In XAML Bindings, Avalonia Properties should be in parentheses. Here is an example of a binding's path in XAML: `MyProp1.(local:AttachedProperties.AttachedProperty1).MyProp2`. This path means find regular C\# property `MyProp1` on the source object, then find attached property `AttachedProperty1` \(defined within class `AttachedProperties` of local namespace\) within the object returned by the first link, and then find the regular C\# property `MyProp2` within that attached property value.
4. **Target property** - can only be one of Attached, Style or Direct Property kinds.
5. **Binding Mode** can be:
   1. `OneWay` - from source to target
   2. `TwoWay` - when either source or target change, the other will also get updated.
   3. `OneWayToSource` - when target is updated, the source is also updated, but not vice versa.
   4. `OneTime` - syncs the target from the source only once - during the initialization.
   5. `Default` - relies on the target property's preferred binding mode. When Attached Style or Direct Property is initialized, one can specify the preferred binding mode, which will be used in this case \(when `BindingMode` is not specified within the binding itself\).
6. **Converter** - only needed if source and target value are different. It is used to convert values from source to target and vice versa. For usual bindings, the converter should implement `IValueConverter` interface.

There is also a so called `MultiBinding`, both in Avalonia and in WPF. `MultiBinding` assumes multiple binding sources and still the same single binding target. The multiple sources are combined into a single target by a special converter that implements `IMultiValueConverter`.

One of the complex parts of the binding is that there are several ways to specify the source object both in Avalonia and in WPF but Avalonia has more ways to do it. Here is the description of various methods of specifying the source object:

1. If you do not specify the source object at all - in that case, the default source object will be given by `DataContext` property of the `Binding`'s target object. `DataContext` automatically propagates down the visual tree unless explicitly changed \(with some exceptions\).
2. You can specify the source explicitly in XAML by assigning it to the binding's `Source` property. You can assign it directly in C\#, or in XAML one can use `StaticResource` and `x:Static` markup extensions.
3. There is `ElementName` property that can be used to find the source element within the same XAML file by name.
4. There is `RelativeSource` property that opens up several more interesting ways of locating the source object depending on its `Mode` property:
   1. For `Mode==Self`, the source object will be the same as the target object.
   2. `Mode==TemplatedParent` can only be used within a `ControlTemplate` of some Avalonia `TemplatedControl` - the explanation of what it means will be given in a future guide explaining the control templates. `TemplatedParent` within the control template means that the source of the binding is the control for which the template is used.
   3. `Mode==FindAncestor` means that the source object will be searched for up the Visual Tree. `AncestorType` property should also be used in this mode, to specify the type of the source object to search. If nothing else is specified, the first object of that type will become the source object. If also `AncestorLevel` is set to some positive integer `N`, it specifies that the `N`th ancestor object of that type will be returned \(by default `AncestorLevel == 1`\) as the source for the binding. In Avalonia \(but not in WPF\), the `RelativeSource`'s `Tree` property can be \(amazingly\) set to `TreeType.Logical` \(it is `TreeType.Visual` by default\). In that case, the ancestors are being searched up the Logical tree \(which is sparser and less complex\).

Now enough theory, let us do some practical examples.

### **Demonstrating Different Binding Sources in XAML**

This sample is located within [NP.Demos.BindingSourcesSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BindingSourcesSample) solution. This sample shows various possible ways to set the source of the bindings in XAML.

Here is what you see once you run the sample:

![](https://www.codeproject.com/KB/Articles/5311995/BindingSourcesSample.png)

Now let us go one by one over the various samples \(all of which are located in _MainWindow.axaml_ file\) and explain the XAML code that produced it.

#### **DataContext \(default\) Binding Source**

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        ...
        DataContext="This is the Window's DataContext"
        ...>
        ...
        <Grid ...>
            <TextBlock Text="{Binding}"/>
        </Grid>
        ...
</Window>
```

When no source is specified in the binding, the `Binding`'s source reverts to the element's `DataContext` property. In our sample, the `DataContext` is set on the Window, but since it propagates down the visual tree \(unless explicitly changed\) - our `TextBlock` has the same `DataContext` - which is simply a `string` that is displayed by our `TextBlock`.

#### **Setting the Binding.Source Property**

In our second example, we employ the `StaticResource` markup extension to set the source of the binding to the string "This is the Window's resource" defined as the resource of the `Window`:

```markup
<Window xmlns="https://github.com/avaloniaui"
        ...>
  <Window.Resources>
    <x:String x:Key="TheResource">This is the Window's resource</x:String>
  </Window.Resources>
  ...
        <TextBlock Text="{Binding Source={StaticResource TheResource}}"
                   .../>
  ...
</Window>  
```

#### **Binding by ElementName**

Our window has XAML name - "`TheWindow`" and we use it to bind to its `Tag`: \(`Tag` is a property defined on every Avalonia Control that can contain any object in it\)

```markup
<Window ...
        Tag="This is the Window's Tag"
        x:Name="TheWindow"
        ...>
        ...
             <TextBlock Text="{Binding Path=#TheWindow.Tag}"
                        .../>
        ...      
</Window>   
```

The above is an Avalonia shorthand for `Text={Binding Path=Tag, ElementName=TheWindow}`.

Note that in many cases "Path=" can be skipped, so that the above binding can be rewritten as 

```markup
<TextBlock Text="{Binding #TheWindow.Tag}"
                        .../>
```

#### **Binding to Self using RelativeSource**

This sample show how element can have itself as `Binding`'s source object using `RelativeSource` in Self mode.

```markup
<TextBlock Text="{Binding Path=Tag, RelativeSource={RelativeSource Self}}"
           Tag="This is my own (TextBox'es) Tag"
         .../>  
```

#### **Binding to the TemplatedParent**

`RelativeSource` with `TemplatedParent` mode should only be used inside a `ControlTemplate` and using it means that the binding refers to a property \(or a path\) defined on the control that is implemented by the current template:

```markup
<TemplatedControl Tag="This is Control's Tag"
                  ...>
    <TemplatedControl.Template>
        <ControlTemplate>
            <TextBlock Text="{Binding Path=Tag, 
                                      RelativeSource={RelativeSource TemplatedParent}}"/>
        </ControlTemplate>
    </TemplatedControl.Template>
</TemplatedControl>  
```

The code above means that we are binding to the `Tag` property on the `TemplatedControl` that is implemented by the `ControlTemplate`.

#### **Binding to a Visual Tree Ancestor using RelativeSource with AncestorType**

Specifying the `AncestorType` will signify to the `Binding` that the `RelativeSource` is in `FindAncestor` mode.

```markup
<Grid ...
    Tag="This is the first Grid ancestor tag"
    ...>
    <StackPanel>
        <TextBlock Text="{Binding Path=Tag, 
                                  RelativeSource={RelativeSource AncestorType=Grid}}"/>
    </StackPanel>
</Grid>
```

#### **Binding to a Visual Tree Ancestor using RelativeSource with AncestorType and AncestorLevel**

Using `AncestorLevel`, you can specify that you need not the first ancestor of the needed type but the Nth - where N can be any positive integer.

In the code below, we are searching for the second `Grid` among the element's ancestors:

```markup
<Grid ...
      Tag="This is the second Grid ancestor tag">
    <StackPanel>
        <Grid Tag="This is the first Grid ancestor tag">
            <StackPanel>
                <TextBlock Text="{Binding Path=Tag, 
                 RelativeSource={RelativeSource AncestorType=Grid, AncestorLevel=2}}"/>
            </StackPanel>
        </Grid>
    </StackPanel>
</Grid>
```

#### **Using Avalonia Binding Path Shorthand to find the Parent in the Logical Tree**

```markup
<Grid Tag="This is the first Grid ancestor tag">
  <StackPanel Tag="This is the immediate ancestor tag">
      <TextBlock Text="{Binding $parent.Tag}"/>
  </StackPanel>
</Grid>  
```

Note that `$parent.Tag` means find the parent \(first ancestor\) of the element and get the `Tag` property from it. This binding should be equivalent to a longer version:

```markup
<TextBlock Text="{Binding Path=Tag,
                          RelativeSource={RelativeSource Mode=FindAncestor, Tree=Logical}}">
```

#### **Using Avalonia Binding Path Shorthand to find the first Parent of type Grid in the Logical Tree**

```markup
<Grid Tag="This is the first Grid ancestor tag">
  <StackPanel Tag="this is the immediate ancestor tag">
    <Button Tag="This is the first logical tree ancestor tag">
      <TextBlock Text="{Binding $parent[Grid].Tag}"/>
    </Button>
  </StackPanel>
</Grid>  
```

`$parent[Grid].Tag` does the trick.

#### **Binding to the Second Ancestor Grid in the Logical Tree Using Avalonia Binding Path Shorthand**

```markup
<Grid Tag="This is the second Grid ancestor tag">
  <StackPanel>
    <Grid Tag="This is the first Grid ancestor tag">
      <StackPanel Tag="this is the immediate ancestor tag">
        <Button Tag="This is the first logical tree ancestor tag">
          <TextBlock Text="{Binding $parent[Grid;1].Tag}"/>
        </Button>
      </StackPanel>
    </Grid>
  </StackPanel>
</Grid>  
```

`$parent[Grid;1]` refers to the second ancestor of type `Grid`. There is an inconsistency here - the numeration of ancestors starts with 1 in the Visual Tree, but with 0 in the Logical Tree.

### **Demonstrating Different Binding Modes**

This sample is located under [NP.Demos.BindingModesSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BindingModesSample) solution. All the code for this sample is located within MainWindow.axaml file.

Run the sample and here is what you'll see:

![](https://www.codeproject.com/KB/Articles/5311995/BindingModes.png)

The top three `TextBoxes` are bound to the same `Tag` property of the `Window` - the first using `TwoWay` mode, the second - `OneWay` and the third - `OneTime`. Try typing in the top `TextBox`. Then, the second from the top `TextBox` will get updated, but not the third:

![](https://www.codeproject.com/KB/Articles/5311995/BindingModesAfterTypingInTheTopBox.png)

This is understandable, since the top `TextBox` has a `TwoWay` binding with the `Window's` tag - when you modify its text, the `Window`'s tag also gets updated and the one way binding to the same tag will update the second `TextBox`.

Nothing will happen if you try to modify text in the second `TextBox` since it has a `OneWay` - from `Window`'s `Tag` to `TextBox.Text` binding. And of course, nothing will happen when someone modifies the text in the 3rd `TextBox`.

Here is the relevant code for the top three text boxes \(the 4th one is special and I'll explain - why - in a moment\):

```markup
<Window Tag="Hello World!"
        ...>
    ...
    <TextBox ...
             Text="{Binding $parent[Window].Tag, Mode=TwoWay}"/>
    <TextBox ...
             Text="{Binding $parent[Window].Tag, Mode=OneWay}"/>
    <TextBox ...
             Text="{Binding $parent[Window].Tag, Mode=OneTime}"/>
    ...
</Window>
```

The forth `TextBox` demonstrates the `OneWayToSource` mode. Note that initially, it does not display anything. If you start typing it in, you'll see that the same text appears below:

![Image 19](https://www.codeproject.com/KB/Articles/5311995/OneWayToSourceBinding.png)

Here is the relevant code for the fourth `TextBox`:XAMLCopy Code

```text
<Grid ...
      Tag="This is a OneWayToSource Grid Tag">
  ...
  <TextBox Text="{Binding $parent[Grid].Tag, Mode=OneWayToSource}"
           .../>
  <TextBlock Text="{Binding $parent[Grid].Tag, Mode=OneWay}"
             .../>
</Grid>  
```

The `TextBox` and the `TextBlock` are both bound to the `Tag` on the `Grid panel`.

Note that the `Tag` originally has some text in it: "This is a `OneWayToSource` Grid Tag". Yet, both the `TextBox` and the `TextBlock` were empty in the beginning. This is because `OneWayToSource` binding removed the initial value of the tag \(the `TextBox` initially did not have any text in it, so it overrode the initial value of the `Tag` because of the binding\).

This is the reason I did not use the Window's `Tag` for the fourth `TextBox` - it would have ruined the initial values for three other `TextBoxes`.

This is also the reason why I rarely if ever use `OneWayToSource` binding - it would have been much more useful if it were assigning the initial value from `Source` to `Target` and only then would be working from `Target` to `Source`.

### **Binding Converters**

Open [NP.Demos.BindingConvertersSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BindingConvertersSample) solution. Here is what you'll see once you run it:

![](https://www.codeproject.com/KB/Articles/5311995/ConvertersSample.png)

Try removing the text from the top `TextBox`. The green text will disappear and the red text will appear instead:

![](https://www.codeproject.com/KB/Articles/5311995/ConvertersTextEmptySample.png)

Also whatever you type in the top or bottom `TextBox`, the same character but inverted from right to left will appear in the other `TextBox`.

Here is the relevant code:

```markup
<Grid ...>
    ...
  <TextBox  x:Name="TheTextBox" 
            Text="Hello World!"
            .../>
  <TextBlock Text="This text shows when the text in the TextBox is empty"
             IsVisible="{Binding #TheTextBox.Text, 
             Converter={x:Static StringConverters.IsNullOrEmpty}}"
             Foreground="Red"
             .../>
  <TextBlock Text="This text shows when the text in the TextBox is NOT empty"
             IsVisible="{Binding #TheTextBox.Text, 
             Converter={x:Static StringConverters.IsNotNullOrEmpty}}"
             Foreground="Green"
             .../>
  <TextBox  Grid.Row="4"
            Text="{Binding #TheTextBox.Text, Mode=TwoWay, 
            Converter={StaticResource TheReverseConverter}}"
            ...>
</Grid>
```

For the two `TextBlock`s, I am using the Avalonia built-in converters - `IsNullOrEmpty` and `IsNotNullOrEmpty`. They are defined as `static` properties within `StringConverters` class which is part of the default Avalonia namespace. This is why, no namespace prefix is necessary and this is why I am using `x:Static` markup extension to find them, e.g., `Converter={x:Static StringConverters.IsNullOrEmpty}`.

The `TextBox` at the bottom is using `ReverseStringConverter` defined in the same project:

```csharp
public class ReverseStringConverter : IValueConverter
{
    private static string? ReverseStr(object value)
    {
        if (value is string str)
        {
            return new string(str.Reverse().ToArray());
        }

        return null;
    }

    public object? Convert
    (object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ReverseStr(value);
    }

    public object? ConvertBack
    (object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ReverseStr(value);
    }
}  
```

Note that the converter implements `IValueConverter` interface. It defines both forward and backward conversion by `Convert(...)` and `ConvertBack(...)` methods correspondingly. The bottom `TextBox`es binding is, of course' `TwoWay` so that whichever `TextBox` changes, the other will change also.

### **MultiValue Binding Sample**

The next sample shows how to connect a target of a binding to multiple sources. The code is located under [NP.Demos.MultiBindingSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.MultiBindingSample) solution.

Run the sample, here is what you'll see:

![](https://www.codeproject.com/KB/Articles/5311995/MultiBindingSample.png)

Try typing `smth` in either of the `TextBox`es. Their concatenation will keep showing at the bottom.

Here is the relevant code that does it:

```markup
<Grid RowDefinitions="Auto,Auto,Auto"
      <TextBox x:Name="Str1"
               Text="Hi"
               .../>
      <TextBox x:Name="Str2" 
               Text="Hello"
               .../>
      <TextBlock ...>
          <TextBlock.Text>
              <MultiBinding Converter="{x:Static local:ConcatenationConverter.Instance}">
                  <Binding Path="#Str1.Text"/>
                  <Binding Path="#Str2.Text"/>
              </MultiBinding>
          </TextBlock.Text>
      </TextBlock>
</Grid>  
```

The `MultiBinding` contains two single value bindings to the individual text boxes:

```markup
<Binding Path="#Str1.Text"/>
<Binding Path="#Str2.Text"/>
```

Their values are converted by the `MultiValue` converter \(`Converter="{x:Static local:ConcatenationConverter.Instance}"`\) into their concatenation.

The `MultiValue` converter is defined in `ConcatenationConverter` class within the sample project:

```csharp
public class ConcatenationConverter : IMultiValueConverter
{
    // static instance to reference
    public static ConcatenationConverter Instance { get; } =
        new ConcatenationConverter();

    public object? Convert(IList<object> values, 
           Type targetType, object parameter, CultureInfo culture)
    {
        if (values == null || values.Count == 0)
        {
            return null;
        }

        return 
            string.Join("", values.Select(v => v?.ToString()).Where(v => v != null));
    }
}  
```

The class implements `IMultiValueConverter` interface \(and not `IValueConverter` used for the single value binding conversion\).

`IMultiValueConverter` has only one method - `Convert(...)` for forward conversion and the first argument to it is the `IList<object>` that has an entry for each source value.

To avoid polluting XAML code by creating a XAML resource, I created a `static` property called `Instance` that refers to a global instance of the same class and is easily accessible from XAML with `x:Static` markup extension: `Converter="{x:Static local:ConcatenationConverter.Instance}"`.

### **Creating a Binding in C\# Code**

The next sample is located under [NP.Demos.BindingInCode](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BindingInCodeSample) solution. Here is what you'll see once you run it:

![](https://www.codeproject.com/KB/Articles/5311995/BindingInCodeSample.png)

Try changing the text in the `TextBox` - nothing else should happen until you press button "**Bind**". Once you press it, the text appears below the `TextBox` mimicking the text within it:

![](https://www.codeproject.com/KB/Articles/5311995/BindingInCodeSampleAfterBind.png)

When you press Button "**Unbind**", the text below will stop reacting to the modifications again.

This functionality is achieved mostly by the code in MainWindow.axaml.cs. The XAML code simply defines and positions the `TextBox` and the `TextBlock` under it as well as the two buttons: `BindButton` and `UnbindButton`:

```markup
...
<StackPanel ...>
    <TextBox x:Name="TheTextBox"
             Text="Hello World"/>
    <TextBlock x:Name="TheTextBlock"
               HorizontalAlignment="Left"/>
</StackPanel>
...
<StackPanel ...>
    <Button x:Name="BindButton" 
            Content="Bind"/>

    <Button x:Name="UnbindButton"
            Content="Unbind"/>
</StackPanel>
...  
```

And here is the relevant C\# code:

```csharp
public partial class MainWindow : Window
{
    TextBox _textBox;
    TextBlock _textBlock;
    public MainWindow()
    {
        InitializeComponent();
        ...
        _textBox = this.FindControl<TextBox>("TheTextBox");
        _textBlock = this.FindControl<TextBlock>("TheTextBlock");

        Button bindButton = this.FindControl<Button>("BindButton");
        bindButton.Click += BindButton_Click;

        Button unbindButton = this.FindControl<Button>("UnbindButton");
        unbindButton.Click += UnbindButton_Click;
    }

    IDisposable? _bindingSubscription;
    private void BindButton_Click(object? sender, RoutedEventArgs e)
    {
        if (_bindingSubscription == null)
        {
            _bindingSubscription =
                _textBlock.Bind(TextBlock.TextProperty, 
                                new Binding { Source = _textBox, Path = "Text" });

            // The following line will also do the trick, but you won't be able to unbind.
            //_textBlock[!TextBlock.TextProperty] = _textBox[!TextBox.TextProperty];
        }
    }

    private void UnbindButton_Click(object? sender, RoutedEventArgs e)
    {
        _bindingSubscription?.Dispose();
        _bindingSubscription = null;
    }

    private void InitializeComponent()
    {
        AvaloniaXamlLoader.Load(this);
    }
}
```

The binding is achieved by calling method `Bind` on the `TextBlock`:

```csharp
_bindingSubscription =
  _textBlock.Bind(TextBlock.TextProperty, new Binding { Source = _textBox, Path = "Text" });
```

It returns a disposable object stored in `_bindingSubscription` field.

In order to destroy the binding - this object must be disposed of: `_bindingSubscription.Dispose()`.

Amazingly enough \(at least for the very truly yours\), the following C\# code will also establish the same binding:

```csharp
_textBlock[!TextBlock.TextProperty] = _textBox[!TextBox.TextProperty];  
```

Only such binding will not be destructible \(or at least not as easily destructible as the one returned by the `Bind(...)` method\).

After a bit of research, I understood how this is working: bang \(!\) operator turns an `AvaloniaProperty` object into an object of type `IndexerDescriptor`. This object can be passed to `AvaloniaObject's` operator `[]` to return an object of type `IBinding`. Then assignment to an `IndexerDescriptor` cell on another `AvaloniaObject` will call the `Bind(...)` method and create the binding.

### **Binding to Properties of Non-Visual Classes**

Previously, we showed different ways to bind two \(source and target\) properties on visual objects. Yet, the source of the binding does not have to be defined on a visual object. In fact, as we mentioned before under the very important and popular MVVM pattern, the complex Visual Objects are being made to mimic behaviors of simple non-visual objects - the so called `ViewModel`s.

In this section, we will show how to create bindable properties in the non-visual classes and bind our visuals to them.

The project is located in [NP.Demos.BindingToNonVisualSample.](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BindingToNonVisualSample) Here is what you see when you run it:

![](https://www.codeproject.com/KB/Articles/5311995/BindingToNonVisualSample1.png)

There is a list of names in the middle. The number of names is displayed on the bottom left and the button to remove last name is on the bottom right.

Click the button to remove the last item on the list. You'll see that the list and the number of items will get updated. When you remove all the items from the list, the "**Number of Items**" will become '`0`' and button will get disabled:

![Image 26](https://www.codeproject.com/KB/Articles/5311995/BindingToNonVisualSample2.png)

The custom code for this sample is located in three files: ViewModel.cs, MainWindow.axaml and MainWindow.axaml.cs. `ViewModel` is a very simple purely non-visual class. Here is its code:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler? PropertyChanged;
    protected void OnPropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }

    // collection of names
    public ObservableCollection<string> Names { get; } = new ObservableCollection<string>();

    // number of names
    public int NamesCount => Names.Count;

    // true if there are some names in the collection,
    // false otherwise
    public bool HasItems => NamesCount > 0;

    public ViewModel()
    {
        Names.CollectionChanged += Names_CollectionChanged;
    }

    // fire then notifications every time Names collection changes.
    private void Names_CollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        // Change Notification for Avalonia for properties
        // NamesCount and HasItems
        OnPropertyChanged(nameof(NamesCount));
        OnPropertyChanged(nameof(HasItems));
    }
}  
```

Note that the collection `Names` is of type `ObservableCollection<string>`. This ensures that the visual collection bound to the `Names` collection will be able to update itself when items are added or removed from the non-visual `Names` collection.

Note also that every time the `Names` collection changes, we are firing `PropertyChanged` events passing to them `nameof(NamesCount)` and `nameof(HasItems)` as arguments. This will notify the bindings to those properties that they have to update their targets.

Now take a look at MainWindow.axaml:

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:NP.Demos.BindingToNonVisualSample"
        ...>
  <!-- Define the DataContext of the Window-->
  <Window.DataContext>
    <local:ViewModel>
      <local:ViewModel.Names>
        <x:String>Tom</x:String>
        <x:String>Jack</x:String>
        <x:String>Harry</x:String>
      </local:ViewModel.Names>
    </local:ViewModel>
  </Window.DataContext>
  <Grid ...>

    <!-- Binding the Items of ItemsControl to the Names collection -->
    <ItemsControl Items="{Binding Path=Names}"
                  .../>

    <Grid Grid.Row="1">

      <!-- Binding Text to NamesCount -->
      <TextBlock Text="{Binding Path=NamesCount, StringFormat='Number of Items: {0}'}"
                 HorizontalAlignment="Left"
                 VerticalAlignment="Center"/>

      <!-- Binding Button.IsEnabled to HasItems -->
      <Button x:Name="RemoveLastItemButton"
              Content="Remove Last Item"
              IsEnabled="{Binding Path=HasItems}"
              .../>
    </Grid>
  </Grid>
</Window>
```

The Window `DataContext` is set directly to contain an object of `ViewModel` type with its `Names` collection populated to `Top`, `Jack` and `Harry`. Since `DataContext` propagates down the Visual Tree, the rest of the elements within the MainWindow.axaml file will have the same `DataContext`.

`ItemControl's` `Items` property is bound to the `Names` collection of the `ViewModel` object: `<ItemsControl Items="{Binding Path=Names}"`. Note that in WPF, the `ItemsControl` would be using `ItemsSource` property instead.

The `TextBlock's` `Text` property is bound to the `NamesCount` property of the `ViewModel`: `<TextBlock Text="{Binding Path=NamesCount, StringFormat='Number of Items: {0}'}"`. Note the use of `StringFormat` within the binding - it allows to add some `string` around the bound value.

Finally, the `Button's` `IsEnabled` property is bound to `HasItems` property on the `ViewModel`, so that the number of items becomes '`0`', the button becomes disabled.

Finally, MainWindow.xaml.cs file simply contains sets the event handler to remove the last item from the `Names` collection on each button click:

```csharp
public MainWindow()
{
    InitializeComponent();

    ...

    Button removeLastItemButton =
        this.FindControl<Button>("RemoveLastItemButton");

    removeLastItemButton.Click += RemoveLastItemButton_Click;
}

private void RemoveLastItemButton_Click(object? sender, RoutedEventArgs e)
{
    ViewModel viewModel = (ViewModel)this.DataContext!;

    viewModel.Names.RemoveAt(viewModel.Names.Count - 1);
```

## Routed Events

### Routed Event Concepts

Same as WPF, Avalonia has a concept of Attached Routed Events that propagate up and down the Visual Tree. They are more powerful and easier to deal with than WPF Routed Events \(as will be explained below\).

Unlike the usual C\# events they

1. Can be defined outside of the class that fires them and 'Attached' to the objects.
2. Can propagate up and down the WPF visual tree - in the sense that the event can be fired by one tree node and handled on another tree node \(one of the firing node's ancestors\).

There are 3 different modes of propagation for the routed events:

1. Direct - this means that the event can only be handled on the same visual tree node that fires it.
2. Bubbling - the event travels from the current node \(the node that raises the event\) to the root of the visual tree and can be handled anywhere on the way. For example, if the visual tree consists of the Window containing a Grid containing a Button and a bubbling event is fired on the Button, then this event will travel from the Button to the Grid and then to the Window.
3. Tunneling - the event travels from the root node of the visual tree to the current node \(the node that raises the event\). Using the same example as above, the Tunneling event will first be raised on the Window, then on the Grid and finally on the button.

The following pictures depict bubbling and tunneling event propagation:

![](../.gitbook/assets/image%20%2855%29.png)



![](../.gitbook/assets/image%20%2845%29.png)

The Avalonia routed events are more powerful and logical than their WPF counterparts because in WPF the event has to choose only one of the routing strategies - it can either be direct or bubbling or tunneling. In order to allow some preprocessing before handing the main \(usually bubbling\) events many bubbling events have their tunneling peers firing before them - the so called Preview events. The preview events are completely different events in WPF and there is no logical connection \(aside from their names\) between them and the corresponding bubbling events. 

In Avalonia, the same event can be registered to have multiple routing strategies - the so called Preview events are no longer necessary - since the same event can be first raised as a tunneling event \(used for preview\) and then as the bubbling event - doing the real stuff. 

Do not worry if you are not a WPF expert and a bit confused about this discussion - we are going to provide simple examples to illustrate what was stated above.

### Built-In Routed Event Example

There are many Routed Events that already exist in Avalonia \(as there are many built-in events in WPF\). We will demonstrate the routing event propagation by using `PointerPressedEvent` routed event, which fires when a user presses mouse button on some visual element in Avalonia. WPF `LeftMouseButtonDown` routed event is very similar to `PointerPressedEvent`.

The sample code is located under [NP.Demos.BuiltInRoutedEventSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.BuiltInRoutedEventSample) solution. 

Take a look at the very simple MainWindow.axaml file:

```markup
<Window x:Name="TheWindow" 
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.BuiltInRoutedEventSample.MainWindow"
        Title="NP.Demos.BuiltInRoutedEventSample"
        Background="Red"
        Width="200"
        Height="200">
  <Grid x:Name="TheRootPanel"
        Background="Green"
        Margin="35">
      <Border x:Name="TheBorder"
              Background="Blue"
              Margin="35"/>
  </Grid>
</Window>
```

We have a `Window` \(with Red background\) containing a `Grid` with Green background containing a `Border` with Blue background.

Run the project in the Visual Studio debugger - here is what you shall see:

![](../.gitbook/assets/image%20%2847%29.png)

Click on the blue square in the middle and take a look a the "Output" pane of the Visual Studio. Here is what you see there:

```csharp
Tunneling Routed Event PointerPressed raised on TheWindow; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheRootPanel; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheRootPanel; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheWindow; Event Source is TheBorder
```

The event travels as the tunneling event from window to the blue border first and then as bubbling event in the opposite direction.

Now take a look at MainWindow.axaml.cs file that contains all the code for handling the event and assigning the handlers:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        ...
        
        // add event handler for the Window
        this.AddHandler
        (
            Control.PointerPressedEvent,
            HandleClickEvent,
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel
            //,true // uncomment if you want to test that the event still propagates event after being handled
        );

        Grid rootPanel = this.FindControl<Grid>("TheRootPanel");

        // add event handler for the Grid
        rootPanel.AddHandler
        (
            Control.PointerPressedEvent, 
            HandleClickEvent,
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel);

        Border border = this.FindControl<Border>("TheBorder");

        // add event handler for the Blue Border in the middle
        border.AddHandler(
            Control.PointerPressedEvent,
            HandleClickEvent,
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel);
    }

    private void HandleClickEvent(object? sender, RoutedEventArgs e)
    {
        Control senderControl = (Control) sender!;

        string eventType = e.Route switch
        {
            RoutingStrategies.Bubble => "Bubbling",
            RoutingStrategies.Tunnel => "Tunneling",
            _ => "Direct"
        };

        Debug.WriteLine($"{eventType} Routed Event {e.RoutedEvent!.Name} raised on {senderControl.Name}; Event Source is {(e.Source as Control)!.Name}");

        // uncomment if you want to test handling the event
        //if (e.Route == RoutingStrategies.Bubble && senderControl.Name == "TheBorder")
        //{
        //    e.Handled = true;
        //}
    }
}
```

We assign the handlers to the Window, Grid and Border by using `AddHandler` method. Let us take a close look at one of them:

```csharp
        // add event handler for the Window
        this.AddHandler
        (
            Control.PointerPressedEvent, // routed event
            HandleClickEvent, // event handler
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel // routing strategy filter
        );
```

First argument to `AddHandler` is the Routed Event - a static object that contains a map of visual objects into event handlers. This is analogous to the AttachedProperty object maintaining a map of visual object into object values. Same as AttachedProperty, the RoutedEvent can be defined outside of the class and will not affect the memory except for objects that have handlers for it.

Second argument is the event handler method `HandleClickEvent`. Here is the method implementation:

```csharp
    private void HandleClickEvent(object? sender, RoutedEventArgs e)
    {
        Control senderControl = (Control) sender!;

        string eventTypeString = e.Route switch
        {
            RoutingStrategies.Bubble => "Bubbling",
            RoutingStrategies.Tunnel => "Tunneling",
            _ => "Direct"
        };

        Debug.WriteLine($"{eventTypeStr} Routed Event {e.RoutedEvent!.Name} raised on {senderControl.Name}; Event Source is {(e.Source as Control)!.Name}");

        ...
    }
```

All it does is just writing the sentence to Debug output \(for Visual Studio Debugger it means that it is writing it into the Output pane\).

Third argument \(`RoutingStrategies.Bubble | RoutingStrategies.Tunnel`\) is the routing strategy filter. For example if you remove `RoutingStrategies.Tunnel` from it it will start reacting only to bubbling event run \(try doing it as an exercise\). By default it is set to `RoutingStrategies.Direct | RoutingStrategies.Bubble`.

Note that all \(or almost all\) built-in Routed Events have their plain C\# event counterparts which are raised when the routed event is raised. We could have used eg. `PointerPressed` C\# event connecting it to `HandleClickEvent` handler:

```csharp
rootPanel.PointerPressed += HandleClickEvent;
```

But in that case we would not be able to choose the `RoutingStrategies` filtering \(it would have remained default - `RoutingStrategies.Direct | RoutingStrategies.Bubble`\). Also we would not be able to choose an important `handledEventsToo` argument which is going to be explained shortly.

At the end of the `HandleClickEvent` method there are several extra commented out lines of code which you should uncomment now:

```csharp
// uncomment if you want to test handling the event
if (e.Route == RoutingStrategies.Bubble && senderControl.Name == "TheBorder")
{
    e.Handled = true;
}
```

The purpose of this code is to set the event to `Handled` once it does all the tunneling and bubbles first time on the border. Try running the application again and click on the Blue border. Here is what will print in the Output pane of the Visual Studio:

```csharp
Tunneling Routed Event PointerPressed raised on TheWindow; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheRootPanel; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
```

Since the event has been handled after the first bubbling on the border, the handlers higher up on the Visual Tree \(those on the Grid and Window\) will not be fired any more. There is however a way to force them to fire even on a routed event that had already been handled. For example, in order to do it on the Window level, uncomment the last argument of `AddHandler(...)` call on the Window:

```csharp
// add event handler for the Window
this.AddHandler
(
    Control.PointerPressedEvent, //routed event
    HandleClickEvent, // event handler
    RoutingStrategies.Bubble | RoutingStrategies.Tunnel // routing strategy filter
    ,true // uncomment if you want to test that the event still propagates event after being handled
);
```

Last argument is called `handledEventsToo` and if it is `true` - it fires the corresponding handler also on events that had been handled before. By default it is `false`.

After uncommenting, run the application again and press the mouse button on the Blue border. Here is what the output is going to be:

```csharp
Tunneling Routed Event PointerPressed raised on TheWindow; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheRootPanel; Event Source is TheBorder
Tunneling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event PointerPressed raised on TheWindow; Event Source is TheBorder
```

The last line show that the bubbling pass of the event was raised on the window, even though the event had been marked as handled before.

Now start the Avalonia Development Tool by mouse-clicking on the window of the sample and pressing **F12**. Click on the "Events" tab and out of all events displayed on the left pane, choose PointerPressed to be checked and undo the check on the rest of them:

![](../.gitbook/assets/image%20%2848%29.png)

After that press on the blue border within the application, an entry for the event will show the main window:

![](../.gitbook/assets/image%20%2846%29.png)

Now mouse-click on the event entry in the main window - the Event Chain pane will show how the event was propagating on the visual tree:

![](../.gitbook/assets/image%20%2843%29.png)

Unfortunately currently, the Event Chain of the tool shows only the propagation of the unhandled event.  It stops showing at the last point when the event was unhandled - in our case the first item of the bubble pass. You can see that there are more instances of the tunneling of our event shown in the tool than in our previous printing. This is because the tool shows all elements within the Visual tree that the event is being raised on, while we only connected the handler to the Window, the Grid and the Border.

### Custom Routed Event Sample

This sample is located within [NP.Demos.CustomRoutedEventSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.CustomRoutedEventSample) solution. It is very similar to the previous sample, only here we fire the custom routed event `MyCustomRoutedEvent` defined within StaticRoutedEvents.cs file:

```csharp
using Avalonia.Interactivity;

namespace NP.Demos.CustomRoutedEventSample
{
    public static class StaticRoutedEvents
    {
        /// <summary>
        /// create the MyCustomRoutedEvent
        /// </summary>
        public static readonly RoutedEvent<RoutedEventArgs> MyCustomRoutedEvent =
            RoutedEvent.Register<object, RoutedEventArgs>
            (
                "MyCustomRouted", 
                RoutingStrategies.Tunnel //| RoutingStrategies.Bubble
            );
    }
}
```

As you see, defining the event is very simple - just calling `RoutedEvent.Register(...)` method passing the name of the event and the routing strategy.

MainWindow.axaml file is exactly the same as in the previous section. MainWindow.axaml.cs code is also very similar to one of the previous section, only here we handle `MyCustomRoutedEvent`, eg:

```csharp
// add event handler for the Window
this.AddHandler
(
    StaticRoutedEvents.MyCustomRoutedEvent, //routed event
    HandleCustomEvent, // event handler
    RoutingStrategies.Bubble | RoutingStrategies.Tunnel // routing strategy filter
);
```

We also add some code to raise the `MyCustomRoutedEvent` when mouse is pressed on the blue border:

```csharp
  // we add the handler to pointer pressed event in order
  // to raise MyCustomRoutedEvent from it.
  border.PointerPressed += Border_PointerPressed;
}

/// PointerPressed handler that raises MyCustomRoutedEvent
private void Border_PointerPressed(object? sender, PointerPressedEventArgs e)
{
    Control control = (Control)sender!;

    // Raising MyCustomRoutedEvent
    control.RaiseEvent(new RoutedEventArgs(StaticRoutedEvents.MyCustomRoutedEvent));
}

```

The code line for raising the event specifically is:

```csharp
// Raising MyCustomRoutedEvent
control.RaiseEvent(new RoutedEventArgs(StaticRoutedEvents.MyCustomRoutedEvent));
```

Here is the \(almost\) full code-behind from MainWindow.axaml.cs file:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        ...
        
        // add event handler for the Window
        this.AddHandler
        (
            StaticRoutedEvents.MyCustomRoutedEvent, //routed event
            HandleClickEvent, // event handler
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel // routing strategy filter
        );

        Grid rootPanel = this.FindControl<Grid>("TheRootPanel");

        // add event handler for the Grid
        rootPanel.AddHandler
        (
            StaticRoutedEvents.MyCustomRoutedEvent, 
            HandleCustomEvent,
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel);

        Border border = this.FindControl<Border>("TheBorder");

        // add event handler for the Blue Border in the middle
        border.AddHandler(
            StaticRoutedEvents.MyCustomRoutedEvent,
            HandleCustomEvent,
            RoutingStrategies.Bubble | RoutingStrategies.Tunnel
            );

        // we add the handler to pointer pressed event in order
        // to raise MyCustomRoutedEvent from it.
        border.PointerPressed += Border_PointerPressed;
    }


    /// PointerPressed handler that raises MyCustomRoutedEvent
    private void Border_PointerPressed(object? sender, PointerPressedEventArgs e)
    {
        Control control = (Control)sender!;

        // Raising MyCustomRoutedEvent
        control.RaiseEvent(new RoutedEventArgs(StaticRoutedEvents.MyCustomRoutedEvent));
    }

    private void HandleCustomEvent(object? sender, RoutedEventArgs e)
    {
        Control senderControl = (Control) sender!;

        string eventTypeStr = e.Route switch
        {
            RoutingStrategies.Bubble => "Bubbling",
            RoutingStrategies.Tunnel => "Tunneling",
            _ => "Direct"
        };

        Debug.WriteLine($"{eventTypeStr} Routed Event {e.RoutedEvent!.Name} raised on {senderControl.Name}; Event Source is {(e.Source as Control)!.Name}");
    }
    ...
}
```

When we run the project and click on the blue square in the middle, the following will be printed onto the Visual Studio Output pane:

```csharp
Tunneling Routed Event MyCustomRouted raised on TheWindow; Event Source is TheBorder
Tunneling Routed Event MyCustomRouted raised on TheRootPanel; Event Source is TheBorder
Tunneling Routed Event MyCustomRouted raised on TheBorder; Event Source is TheBorder
```

Note that only tunneling pass is being handled. This is because we defined the event to be a pure tunneling event by passing its last argument to be `RoutingStrategies.Tunnel`. If we change it to `RoutingStrategies.Tunnel | RoutingStrategies.Bubble`, and restart the solution again, we shall see both tunneling and bubbling passes:

```csharp
Tunneling Routed Event MyCustomRouted raised on TheWindow; Event Source is TheBorder
Tunneling Routed Event MyCustomRouted raised on TheRootPanel; Event Source is TheBorder
Tunneling Routed Event MyCustomRouted raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event MyCustomRouted raised on TheBorder; Event Source is TheBorder
Bubbling Routed Event MyCustomRouted raised on TheRootPanel; Event Source is TheBorder
Bubbling Routed Event MyCustomRouted raised on TheWindow; Event Source is TheBorder
```

## Avalonia Commands and Calling C\# Methods from XAML

### Command Concepts

When someone builds an application it is customary to place the logic controlling the visuals into some non-visual classes \(called View Models\) and then use bindings and other ways of connecting your visuals in XAML to the View Models. The idea behind it is that the non-visual objects are much simpler and easier to test than the Visuals therefore if you deal primarily with non-visual objects, you'll have easier coding and testing. Such pattern is called MVVM.

`Command` is a way to execute some C\# methods within the View Model when a Button or a MenuItem is clicked.

Avalonia `Button` and `MenuItem` each have a property `Command` which can be bound to a `Command` defined within a view model. Such command can execute a View Model method hooked to it. Avalonia does not have its own command implementation, but it is recommended to use ReactiveUI `ReactiveCommand`. One can also control whether the Button \(or a MenuItem\) is enabled or not via a command object placed within the View Model.

Yet such approach of placing the commands within the View Models has major drawbacks:

* It forces the View Models to depend on visual .NET assemblies \(which implement the commands\). This breaks the hard barrier that should be placed between the non-visual View Models and the Visuals. After that it becomes much more difficult to control \(especially on a project with many developers\) that the visual code does not leak into the View Models. 
* It unnecessarily pollutes the View Models.

Avalonia, therefore provides a considerably cleaner way of calling a method on a View Model - by binding the Command to the method's name.

### Using Avalonia Commands for Calling Methods on a View Model

Run this sample located under [NP.Demos.CommandSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.CommandSample) solution. Here is what you'll see:

![](../.gitbook/assets/image%20%2854%29.png)

There is Status field value shown in the middle of the window. When you press "Toggle Status" button it will toggle between `True` and `False`. Clicking "Set Status to True" will set the status value to 'True' and unchecking "Can Toggle Status" checkbox will disable "Toggle Status" button.

Take a look at the file called ViewModel.cs. It contains purely non-visual code. 

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler? PropertyChanged;

    /// <summary>
    /// fires INotifyPropertyChanged.PropertyChanged event
    /// </summary>
    private void OnPropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }


    #region Status Property
    private bool _status;
    /// <summary>
    /// Status notifiable property
    /// </summary>
    public bool Status
    {
        get
        {
            return this._status;
        }
        set
        {
            if (this._status == value)
            {
                return;
            }

            this._status = value;
            this.OnPropertyChanged(nameof(Status));
        }
    }
    #endregion Status Property

    #region CanToggleStatus Property
    private bool _canToggleStatus = true;
    /// <summary>
    /// Controls whether Toggle Status button is enabled or not
    /// </summary>
    public bool CanToggleStatus
    {
        get
        {
            return this._canToggleStatus;
        }
        set
        {
            if (this._canToggleStatus == value)
            {
                return;
            }

            this._canToggleStatus = value;
            this.OnPropertyChanged(nameof(CanToggleStatus));
        }
    }
    #endregion CanToggleStatus Property

    /// <summary>
    /// Toggles the status
    /// </summary>
    public void ToggleStatus()
    {
        Status = !Status;
    }

    /// <summary>
    /// Set the Status to whatever 'status' is passed
    /// </summary>
    public void SetStatus(bool status)
    {
        Status = status;
    }
}
```

 It provides 

* a Boolean property `Status`
* `ToggleStatus()` method that toggles the `Status` property
* `SetStatus(bool status)` method that set `Status` property to whatever argument was passed to it
* `CanToggleStatus` property that controls whether `ToggleStatus()` action is enabled or not.

Whenever any property changes, the `PropertyChanged` event fires, so that the Avalonia bindings will be notified about the property change.

`MainWindow` constructor locаted within MainWindow.axaml.cs file sets the `DataContext` of the Window to be and instance of our `ViewModel` class:

```csharp
public MainWindow()
{
    InitializeComponent();

    ...
    
    this.DataContext = new ViewModel();
}
```

`DataContext` is a special `StyledProperty` that is inherited by the descendants of the visual tree \(unless changed explicitly\), so it will also be the same for the window descendants.

Here is the content of MainWindow.axaml file:

```markup
<Window x:Name="TheWindow" 
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.CommandSample.MainWindow"
        Title="NP.Demos.CommandSample"
        Width="200"
        Height="300">
  <Grid x:Name="TheRootPanel" 
        RowDefinitions="*, *, *, *"
        Margin="20">
    <CheckBox IsChecked="{Binding Path=CanToggleStatus, Mode=TwoWay}"
              Content="Can Toggle Status"
              HorizontalAlignment="Left"
              VerticalAlignment="Center"/>
    
    <TextBlock Text="{Binding Path=Status, StringFormat='Status={0}'}"
               Grid.Row="1"
               HorizontalAlignment="Left"
               VerticalAlignment="Center"/>
    
    <Button  Content="Toggle Status"
             Grid.Row="2"
             HorizontalAlignment="Right"
             VerticalAlignment="Center"
             IsEnabled="{Binding Path=CanToggleStatus}"
             Command="{Binding Path=ToggleStatus}"/>

    <Button  Content="Set Status to True"
             Grid.Row="3"
             HorizontalAlignment="Right"
             VerticalAlignment="Center"
             Command="{Binding Path=SetStatus}"
             CommandParameter="True"/>
  </Grid>
</Window>
```

Checkbox at the top has its `IsChecked` property two-way bound to `CanToggleStatus` of the ViewModel:

```markup
<CheckBox IsChecked="{Binding Path=CanToggleStatus, Mode=TwoWay}"
              Content="Can Toggle Status" 
              .../>
```

so that when it changes, the corresponding property changes also.

TextBlock displays status \(true or false\):

```markup
    <TextBlock Text="{Binding Path=Status, StringFormat='Status={0}'}"
               ... />
```

Top Button \(via its command calls `ToggleStatus()` method on the ViewModel and its `IsEnabled` property is bound to `CanToggleStatus` property on the ViewModel:

```markup
    <Button  Content="Toggle Status"
             ...
             IsEnabled="{Binding Path=CanToggleStatus}"
             Command="{Binding Path=ToggleStatus}"/>
```

The bottom button is there to demonstrate calling a method with an argument on the view model. Its `Command` property is bound to `SetStatus(bool status)` method that has one Boolean argument - `status`. To pass this argument, we set the `CommandParameter` property to "True":

```markup
    <Button  Content="Set Status to True"
             Grid.Row="3"
             HorizontalAlignment="Right"
             VerticalAlignment="Center"
             Command="{Binding Path=SetStatus}"
             CommandParameter="True"/>
```

## Avalonia User Controls

User controls is something that **almost never should be created or used** since for controls, lookless \(also called custom\) controls are more powerful and have better separation between visual and non-visual concerns and the for Views of the MVVM pattern, `DataTemplates` are better.

Yet, the Avalonia tale will not be complete unless we speak about `UserControls`. They are also the easiest to create and understand. 

The sample's code is located under [NP.Demos.UserControlSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.UserControlSample) solution:

It contains `MyUserControl` user control:

![](../.gitbook/assets/image%20%2844%29.png)

To create such UserControl from scratch - use Add-&gt;New Item context menu and then, in the opened dialog, choose Avalonia on the left and "User Control \(Avalonia\)" on the right and then press Add button. 

![Creating User Control](../.gitbook/assets/image%20%2851%29.png)

Run the sample, here is the window that pops up:

![](../.gitbook/assets/image%20%2850%29.png)

Start typing within the `TextBox`. Buttons "Cancel" and "Save" will become enabled. If you press Cancel, the text will revert to the saved value \(in the beginning it is empty\). If you press Save, the new saved value will become whatever currently is in the `TextBox`. The buttons "Cancel" and "Save" are disabled when the Entered text is the same as the Saved Text and are enabled otherwise:

![](../.gitbook/assets/image%20%2852%29.png)

MainWindow.axaml file has only one non-trivial element: `MyUserControl`:

```markup
<Window x:Name="TheWindow"
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.UserControlSample.MainWindow"
        xmlns:local="clr-namespace:NP.Demos.UserControlSample"
        ...>
  <local:MyUserControl Margin="20"/>
</Window>
```

MainWindow.axaml.cs file does not have any non-default code, so all the code for this functionality is located within MyUserControl.axaml and MyUserControl.axaml.cs files - the C\# file is just the code behind for the XAML file. 

Here is the content of MyUserControl.axaml file:

```markup
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="NP.Demos.UserControlSample.MyUserControl">
  <Grid RowDefinitions="Auto, Auto, *, Auto">
    <StackPanel Orientation="Horizontal"
                HorizontalAlignment="Left"
                VerticalAlignment="Center">
      <TextBlock Text="Enter Text: "
                 VerticalAlignment="Center"/>
      <TextBox x:Name="TheTextBox"
                 MinWidth="150"/>
    </StackPanel>
    <StackPanel Orientation="Horizontal"
                HorizontalAlignment="Left"
                VerticalAlignment="Center"
                Grid.Row="1"
                Margin="0,10">
      <TextBlock Text="Saved Text: "
                 VerticalAlignment="Center"/>
      <TextBlock x:Name="SavedTextBlock"/>
    </StackPanel>
    <StackPanel Orientation="Horizontal"
                HorizontalAlignment="Right"
                Grid.Row="3">
      <Button x:Name="CancelButton"
              Content="Cancel"
              Margin="5,0"/>
      <Button x:Name="SaveButton" 
              Content="Save"
              Margin="5,0"/>
    </StackPanel>
  </Grid>
</UserControl>
```

It has no bindings, and no commands - just a passive arrangement of various Visual elements.

The functionality that makes it all work is located in the code-behind file MyUserControl.axaml.cs:

```csharp
public partial class MyUserControl : UserControl
{
    private TextBox _textBox;
    private TextBlock _savedTextBlock;
    private Button _cancelButton;
    private Button _saveButton;

    // saved value is retrieved from and saved to
    // the _savedTextBlock
    private string? SavedValue
    {
        get => _savedTextBlock.Text;
        set => _savedTextBlock.Text = value;
    }

    // NewValue is retrieved from and saved to
    // the _textBox
    private string? NewValue
    {
        get => _textBox.Text;
        set => _textBox.Text = value;
    }

    public MyUserControl()
    {
        InitializeComponent();

        // set _cancelButton and its Click event handler
        _cancelButton = this.FindControl<Button>("CancelButton");
        _cancelButton.Click += OnCancelButtonClick;

        // set _saveButton and its Click event handler
        _saveButton = this.FindControl<Button>("SaveButton");
        _saveButton.Click += OnSaveButtonClick;

        // set the TextBlock that contains the Saved text
        _savedTextBlock = this.FindControl<TextBlock>("SavedTextBlock");

        // set the TextBox that contains the new text
        _textBox = this.FindControl<TextBox>("TheTextBox");

        // initial New and Saved values should be the same
        NewValue = SavedValue;

        // every time the text changes, we should check if
        // Save and Cancel buttons should be enabled or not
        _textBox.GetObservable(TextBox.TextProperty).Subscribe(OnTextChanged);
    }

    // On Cancel, the TextBox value should become the same as SavedValue
    private void OnCancelButtonClick(object? sender, RoutedEventArgs e)
    {
        NewValue = SavedValue;
    }

    // On Save, the Saved Value should become the same as the TextBox Value
    private void OnSaveButtonClick(object? sender, RoutedEventArgs e)
    {
        SavedValue = NewValue;

        // also we should reset the IsEnabled states of the buttons
        OnTextChanged(null);
    }


    private void OnTextChanged(string? obj)
    {
        bool canSave = NewValue != SavedValue;

        // _cancelButton as _saveButton are enabled if TextBox'es value
        // is not the same as saved value and disabled otherwise.
        _cancelButton.IsEnabled = canSave;
        _saveButton.IsEnabled = canSave;
    }

    private void InitializeComponent()
    {
        AvaloniaXamlLoader.Load(this);
    }
}
```

The visual elements defined within MyUserControl.xaml file are obtained inside the C\# code by using `FindControl<TElement>("ElementName")` method, eg.

```csharp
// set _cancelButton and its Click event handler
_cancelButton = this.FindControl<Button>("CancelButton");
```

Then the button's Click event is assigned a handler, eg:

```csharp
_cancelButton.Click += OnCancelButtonClick;
```

All the interesting processing is done inside the `Click` event handlers and inside the TextBox'es Text Change observable subscription:

```csharp
// every time the text changes, we should check if
// Save and Cancel buttons should be enabled or not
_textBox.GetObservable(TextBox.TextProperty).Subscribe(OnTextChanged);
```

The main problem with the UserControl is that we are tightly coupling the visual representation provided by MyUserControl.axaml file and the C\# logic contained within MyUserControl.axaml.cs file. 

Using Custom Control we can completely separate them as will be shown below.

Also, the visual representation from the C\# logic can be separated using View-ViewModel part of MVVM pattern, so that one could use completely different visual representations \(furnished by different DataTemplates\) with the same View Model that defines the business logic. Such MVVM example will be given in a future chapter.

## Avalonia ControlTemplates and CustomControls

You can find this sample under [NP.Demos.CustomControlSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.CustomControlSample) solution. The sample behaves in exactly the same way as the previous sample, but is built very differently. All non-default C\# functionality is located under MyCustomControl.cs file:

![](../.gitbook/assets/image%20%2856%29.png)

Here is its code:

```csharp
public class MyCustomControl : TemplatedControl
{
    #region NewValue Styled Avalonia Property
    public string? NewValue
    {
        get { return GetValue(NewValueProperty); }
        set { SetValue(NewValueProperty, value); }
    }

    public static readonly StyledProperty<string?> NewValueProperty =
        AvaloniaProperty.Register<MyCustomControl, string?>
        (
            nameof(NewValue)
        );
    #endregion NewValue Styled Avalonia Property


    #region SavedValue Styled Avalonia Property
    public string? SavedValue
    {
        get { return GetValue(SavedValueProperty); }
        set { SetValue(SavedValueProperty, value); }
    }

    public static readonly StyledProperty<string?> SavedValueProperty =
        AvaloniaProperty.Register<MyCustomControl, string?>
        (
            nameof(SavedValue)
        );
    #endregion SavedValue Styled Avalonia Property


    #region CanSave Direct Avalonia Property
    private bool _canSave = default;

    public static readonly DirectProperty<MyCustomControl, bool> CanSaveProperty =
        AvaloniaProperty.RegisterDirect<MyCustomControl, bool>
        (
            nameof(CanSave),
            o => o.CanSave
        );

    public bool CanSave
    {
        get => _canSave;
        private set
        {
            SetAndRaise(CanSaveProperty, ref _canSave, value);
        }
    }

    #endregion CanSave Direct Avalonia Property

    // CanSave is set to true when SavedValue is not the same as NewView
    // false otherwise
    private void SetCanSave(object? _)
    {
        CanSave = SavedValue != NewValue;
    }

    public MyCustomControl()
    {
        this.GetObservable(NewValueProperty).Subscribe(SetCanSave);
        this.GetObservable(SavedValueProperty).Subscribe(SetCanSave);
    }

    public void Save()
    {
        SavedValue = NewValue;
    }

    public void Cancel()
    {
        NewValue = SavedValue;
    }
}
```

Do not be scared by the number of lines, most of the code is there because of the StyledProperty and DirectProperty definitions and was created by the snippets _avsp_ and _avdr_ described in [Avalonia Special Properties](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/building-avalonia-applications/introduction-to-avalonia-wpf-concepts#avalonia-special-properties) chapter and available at [NP.Avalonia.Visuals/AvaloniaSnippets](https://github.com/npolyak/NP.Avalonia.Visuals/tree/main/AvaloniaSnippets). 

There are two Styled Properties: `NewValue` and `SavedValue` and one Directy Property: `CanSave`. Whenever any of the styled properties change, the direct property is reevaluated to be false if and only if `NewValue == SavedValue`. This is achieved by subscribing to `NewValue` and `SavedValue` changes within the class constructor:

```csharp
public MyCustomControl()
{
    this.GetObservable(NewValueProperty).Subscribe(SetCanSave);
    this.GetObservable(SavedValueProperty).Subscribe(SetCanSave);
}
```

and by setting it within the callback `SetCanSave(...)` method:

```csharp
// CanSave is set to true when SavedValue is not the same as NewView
// false otherwise
private void SetCanSave(object? _)
{
    CanSave = SavedValue != NewValue;
}
```

The unneeded argument to this method is passed in order for its signature to match the one required by `Subscribe(...)` method.

There are also two public methods to be called by the Buttons' commands: `void Save()` and `void Cancel()`:

```csharp
    public void Save()
    {
        SavedValue = NewValue;
    }

    public void Cancel()
    {
        NewValue = SavedValue;
    }
```

The difference between this C\# file and the MyUserControl.axaml.cs file \(which we described in the above section\) is that this file is completely unaware of the XAML implementation and does not have any references to XAML elements.

Instead the XAML built as a ControlTemplate within MainWindow.axaml file refers to the properties and methods defined within MyCustomControl.cs file via bindings and commands.

First of all notice, that we derived our `MyCustomControl` class from `TemplatedControl`:

```csharp
public class MyCustomControl : TemplatedControl
{
}
```

Because of that it has `Template` property of `ControlTemplate` type which we can set to whatever we want. Here is the corresponding XAML code located within MainWindow.axaml file:

```markup
<Window x:Name="TheWindow"
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.CustomControlSample.MainWindow"
        xmlns:local="clr-namespace:NP.Demos.CustomControlSample"
        ...>
  <local:MyCustomControl Margin="20">
    <local:MyCustomControl.Template>
      <ControlTemplate TargetType="local:MyCustomControl">
        <Grid RowDefinitions="Auto, Auto, *, Auto">
          <StackPanel Orientation="Horizontal"
                      HorizontalAlignment="Left"
                      VerticalAlignment="Center">
            <TextBlock Text="Enter Text: "
                       VerticalAlignment="Center"/>
            <TextBox x:Name="TheTextBox"
                     Text="{Binding Path=NewValue, Mode=TwoWay, RelativeSource={RelativeSource TemplatedParent}}"
                     MinWidth="150"/>
          </StackPanel>
          <StackPanel Orientation="Horizontal"
                      HorizontalAlignment="Left"
                      VerticalAlignment="Center"
                      Grid.Row="1"
                      Margin="0,10">
            <TextBlock Text="Saved Text: "
                       VerticalAlignment="Center"/>
            <TextBlock x:Name="SavedTextBlock"
                       Text="{TemplateBinding SavedValue}"/>
          </StackPanel>
          <StackPanel Orientation="Horizontal"
                      HorizontalAlignment="Right"
                      Grid.Row="3">
            <Button x:Name="CancelButton"
                    Content="Cancel"
                    Margin="5,0"
                    IsEnabled="{TemplateBinding CanSave}"
                    Command="{Binding Path=Cancel, RelativeSource={RelativeSource TemplatedParent}}"/>
            <Button x:Name="SaveButton"
                    Content="Save"
                    Margin="5,0"
                    IsEnabled="{TemplateBinding CanSave}"
                    Command="{Binding Path=Save, RelativeSource={RelativeSource TemplatedParent}}"/>
          </StackPanel>
        </Grid>
      </ControlTemplate>
    </local:MyCustomControl.Template>
  </local:MyCustomControl>
</Window>

```

We set the `Template` property to the `ControlTemplate` object via the following lines:

```markup
<local:MyCustomControl Margin="20">
  <local:MyCustomControl.Template>
    <ControlTemplate TargetType="local:MyCustomControl">
    ...
```

Note that we are populating the `Template` property in line - which is good for prototyping, but bad for the reuse. Usually the Control Template is created as a resource in some resource file and then we use `{StaticResource <ResourceKey>}` markup extension to set the `Template` property. So the lines above would like like:

```markup
<local:MyCustomControl Margin="20"
                       Template="{StaticResource MyCustomControlTemplate}">
```

This way we'd be able to re-use the same template for multiple controls. Alternatively, we can place the Control Templates with Styles and use Styles for our custom controls, but this will be explained later in more detail. 

Note that we specify the `TargetType` of the `ControlTemplate`:

```markup
<ControlTemplate TargetType="local:MyCustomControl">
```

This will allow us to connect to the properties defined by `MyCustomControl` class by using `TemplateBinding` or `{RelativeSource TemplatedParent}`.

The `TextBox` is bound to the `NewValue` property of the control in the `TwoWay` mode, so that changes of one will affect the other:

```markup
<TextBox x:Name="TheTextBox"
         Text="{Binding Path=NewValue, Mode=TwoWay, RelativeSource={RelativeSource TemplatedParent}}"
         MinWidth="150"/>
```

"SavedTextBlock" `TextBlock` is bound to `SavedValue`:

```markup
<TextBlock x:Name="SavedTextBlock"
           Text="{TemplateBinding SavedValue}"/>
```

And the buttons' commands are bound to the corresponding public methods: `Cancel()` and `Save()`, while the buttons' `IsEnabled` property is bound to `CanSave` property of the control:

```markup
<Button x:Name="CancelButton"
        Content="Cancel"
        Margin="5,0"
        IsEnabled="{TemplateBinding CanSave}"
        Command="{Binding Path=Cancel, RelativeSource={RelativeSource TemplatedParent}}"/>
<Button x:Name="SaveButton"
        Content="Save"
        Margin="5,0"
        IsEnabled="{TemplateBinding CanSave}"
        Command="{Binding Path=Save, RelativeSource={RelativeSource TemplatedParent}}"/>
```

[NP.Demos.DifferentVisualsForCustomControlSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.DifferentVisualsForCustomControlSample) shows exactly the same custom control displayed in two different ways:

![](../.gitbook/assets/image%20%2863%29.png)

The representation at the top is the same as in the previous sample - while in the bottom I changed the row orders, so that the buttons are at the top, saved text in the middle and TextBox is at the bottom. That would not be possible with the User Control.

Take a look at the code of the sample. Templates for both visual representations are located within Resources.axaml file under Themes project folder. MainWindow.axaml file contains a ResourceInclude for that file and `StaticResource` references to the two implementations - CustomControlTemplate1 and CustomControlTemplate2:

```markup
<Window x:Name="TheWindow"
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.DifferentVisualsForCustomControlSample.MainWindow"
        xmlns:local="clr-namespace:NP.Demos.DifferentVisualsForCustomControlSample"
        ...>
  <Window.Resources>
    <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceInclude Source="avares://NP.Demos.DifferentVisualsForCustomControlSample/Themes/Resources.axaml"/>
      </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
  </Window.Resources>
  <Grid RowDefinitions="*, *">
    <local:MyCustomControl Margin="20"
                           Template="{StaticResource CustomControlTemplate1}"/>
    <local:MyCustomControl Margin="20"
                           Grid.Row="1"
                           Template="{StaticResource CustomControlTemplate2}"/>
  </Grid>
</Window>
```

## DataTemplates and View Models

### Introduction to View / View Models Concepts

MVVM is an abbreviation for Model-View-View Model pattern. 

View is the visuals that determine the look, feel and visual behaviors of the application.

View Model is a completely non-visual class or set of classes that has two major roles:

1. It provides some functionality that the view can mimic or call via bindings, commands or by other means, e.g. behaviors. For example the View Model can have an method `void SaveAction()` and a property `IsSaveActionAllowed`, while the View will have a button  calling `SaveAction()` method whose `IsEnabled` property will be connected to `IsSaveActionAllowed` property on the View Model.
2. It wraps the model \(e.g. data that comes from the backend\), provides notifications to the view if Model changed and vice versa and can also provide the communication functionality between different View Models and models.

Dealing with Avalonia Tutorial we are not interested in communications between the View Model and the Model - this is an important topic which deserves a guide of its own. Instead we shall concentrate here on the View - View Model \(VVM\) part of the MVVM pattern.

 VVM pattern is best achieved in Avalonia by using `ContentPresenter` \(for a single object\) or `ItemsPresenter` for a collection of objects. 

`ContentPresenter` with the help of a `DataTemplate` converts a non-visual object into a visual object \(a view\).

![](../.gitbook/assets/image%20%2859%29.png)

`Content` property of `ContentPresenter` is usually set to a non-visual object, while `ContentTemplate` should be set to a `DataTemplate`. `ContentPresenter` combines them into a visual object \(View\) where the DataContext is given by the `ContentPresenter's` `Content` property while the Visual tree is provided by the `DataTemplate`.

`ItemsPresenter` with the help of a `DataTemplate` converts a collection of non-visual objects into a collection of visual objects, each containing a `ContentPresenter` that converts the individual View Model item within the collection into a Visual object. The Visual objects are arranged according to the panel provided by `ItemsPresenter.ItemsPanel` property value. 

![](../.gitbook/assets/image%20%2860%29.png)

`Items` property of `ItemsPresenter` usually contains a collection of non-visual objects. `ItemTemplate` has `DataTemplate` object in it and `ItemsPresenter` combines them into a collection of Visual objects. 

### `ContentPresenter` Sample

Code for this sample is located in [NP.Demos.ContentPresenterSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.ContentPresenterSample) solution. The demo application behaves in exactly the same fashion as the samples in [Avalonia User Controls](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/building-avalonia-applications/introduction-to-avalonia-wpf-concepts#avalonia-user-controls) and [Avalonia ControlTemplates and CustomControls](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/building-avalonia-applications/introduction-to-avalonia-wpf-concepts#avalonia-controltemplates-and-customcontrols) chapters.

![](../.gitbook/assets/image%20%2861%29.png)

  
Start typing within the `TextBox`. Buttons "Cancel" and "Save" will become enabled. If you press Cancel, the text will revert to the saved value \(in the beginning it is empty\). If you press Save, the new saved value will become whatever currently is in the `TextBox`. The buttons "Cancel" and "Save" are disabled when the Entered text is the same as the Saved Text and are enabled otherwise.

Unlike in previous cases, we are not creating either User or Custom Controls to achieve this. Instead we are using a completely non-visual `ViewModel` and a `DataTemplate` married together by a `ContentPresenter`. 

The non-default code is located within ViewModel.cs and MainWindow.axaml files. 

Here is the content of ViewModel.cs file:

```csharp
using System.ComponentModel;

namespace NP.Demos.ContentPresenterSample
{
    public class ViewModel : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler? PropertyChanged;

        private void OnPropertyChanged(string propName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
        }


        #region SavedValue Property
        private string? _savedValue;
        public string? SavedValue
        {
            get
            {
                return this._savedValue;
            }
            private set
            {
                if (this._savedValue == value)
                {
                    return;
                }

                this._savedValue = value;
                this.OnPropertyChanged(nameof(SavedValue));
                this.OnPropertyChanged(nameof(CanSave));
            }
        }
        #endregion SavedValue Property


        #region NewValue Property
        private string? _newValue;
        public string? NewValue
        {
            get
            {
                return this._newValue;
            }
            set
            {
                if (this._newValue == value)
                {
                    return;
                }

                this._newValue = value;
                this.OnPropertyChanged(nameof(NewValue));
                this.OnPropertyChanged(nameof(CanSave));
            }
        }
        #endregion NewValue Property

        // CanSave is set to true when SavedValue is not the same as NewView
        // false otherwise
        public bool CanSave => NewValue != SavedValue;

        public void Save()
        {
            SavedValue = NewValue;
        }

        public void Cancel()
        {
            NewValue = SavedValue;
        }
    }
}
```

We have `NewValue` and `SavedValue` string properties that fire `PropertyChanged` notification event when either of them is changed. They also notify of possible change of `CanSave` Boolean property that is `true` if and only if they are not the same:

```csharp
// CanSave is set to true when SavedValue is not the same as NewView
// false otherwise
public bool CanSave => NewValue != SavedValue;
```

There are also two public methods for saving and cancelling:

```csharp
public void Save()
{
    SavedValue = NewValue;
}

public void Cancel()
{
    NewValue = SavedValue;
}
```

 MainWindow.axaml file defines the ViewModel instance, the DataTemplate as Resources and the ContentPresenter that marries them. Here is the `ContentPresenter`:

```markup
<Window ...>
    <Window.Resources>
        ...
    </Window.Resources>
    <ContentPresenter Margin="20"
                      Content="{StaticResource TheViewModel}"
                      ContentTemplate="{StaticResource TheDataTemplate}"/>
</Window>
```

The instance of the view model and the data template are assigned to the `Content` and `ContentTemplate` properties of the `ContentPresenter` via `StaticResource` markup extension.

Here is how we define an instance of the `ViewModel` as a Window resource:

```markup
<Window ...>
    <Window.Resources>
        <local:ViewModel x:Key="TheViewModel"/>
        ...
    </Window.Resources>
...
</Window>
```

And here is how we define the DataTemplate:

```markup
<Window ...>
    <Window.Resources>
            <local:ViewModel x:Key="TheViewModel"/>
            <DataTemplate x:Key="TheDataTemplate">
              <Grid RowDefinitions="Auto, Auto, *, Auto">
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Left"
                            VerticalAlignment="Center">
                  <TextBlock Text="Enter Text: "
                              VerticalAlignment="Center"/>
                  <TextBox x:Name="TheTextBox"
                            Text="{Binding Path=NewValue, Mode=TwoWay}"
                            MinWidth="150"/>
                </StackPanel>
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Left"
                            VerticalAlignment="Center"
                            Grid.Row="1"
                            Margin="0,10">
                  <TextBlock Text="Saved Text: "
                              VerticalAlignment="Center"/>
                  <TextBlock x:Name="SavedTextBlock"
                              Text="{Binding Path=SavedValue}"/>
                </StackPanel>
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Right"
                            Grid.Row="3">
                  <Button x:Name="CancelButton"
                          Content="Cancel"
                          Margin="5,0"
                          IsEnabled="{Binding Path=CanSave}"
                          Command="{Binding Path=Cancel}"/>
                  <Button x:Name="SaveButton"
                          Content="Save"
                          Margin="5,0"
                          IsEnabled="{Binding Path=CanSave}"
                          Command="{Binding Path=Save}"/>
                </StackPanel>
              </Grid>
            </DataTemplate>
    </Window.Resources>
...
</Window>
```

Remember that the `ViewModel` object provided as `Content` property to the `ContentPresenter` becomes a `DataContext` for the Visuals created by the DataTemplate, so we can bind the properties on the DataTemplate to the properties of the View Model without specifying the binding's source object \(since `DataContext` is the default source of the binding\).

We bind the `TextBox` to the `NewValue` property of the `ViewModel` in the `TwoWay` mode so that if either changes, the other  would also change:

```markup
<TextBox x:Name="TheTextBox"
         Text="{Binding Path=NewValue, Mode=TwoWay}"
         MinWidth="150"/>
```

We bind the SavedTextBlock's Text property to `SavedValue`:

```markup
<TextBlock x:Name="SavedTextBlock"
           Text="{Binding Path=SavedValue}"/>
```

And we bind the buttons' Commands to the `Save()` and `Cancel()` methods while also binding the buttons' `IsEnabled` property to `CanSave` Boolean property of the `ViewModel`:

```markup
<Button x:Name="CancelButton"
        Content="Cancel"
        Margin="5,0"
        IsEnabled="{Binding Path=CanSave}"
        Command="{Binding Path=Cancel}"/>
<Button x:Name="SaveButton"
        Content="Save"
        Margin="5,0"
        IsEnabled="{Binding Path=CanSave}"
        Command="{Binding Path=Save}"/>
```

Of course we can pull the DataTemplate into a different file and even into a different project and re-use it in many places.

### `ItemsPresenter` Sample

This sample describes how to use `ItemsPresenter` to display a collection of non-visual objects. The sample's code is located in [NP.Demos.ItemsPresenterSample](https://github.com/npolyak/NP.Avalonia.Demos/tree/main/NP.Demos.AvaloniaBasicConcepts/NP.Demos.ItemsPresenterSample) solution.

Run the sample, here is what you'll see:

![](../.gitbook/assets/image%20%2867%29.png)

Try making the window more narrow the name will wrap down eg:

![](../.gitbook/assets/image%20%2866%29.png)

This is because we are using a `WrapPanel` to display multiple items each item containing first and last names of a person.

Press "Remove Last" button and the last person item will be removed and the "Number of People" text will be updated:

![](../.gitbook/assets/image%20%2865%29.png)

Keep pressing the button, until there are no items left - the button "Remove Last" will become disabled:

![](../.gitbook/assets/image%20%2864%29.png)

Take a look a the code for the sample. There are two View Model files added: PersonViewModel.cs and TestViewModel.cs. 

`PersonViewModel` is the simplest possible class containing immutable properties `FirstName` and `LastName`:

```csharp
public class PersonViewModel
{
    public string FirstName { get; }

    public string LastName { get; }

    public PersonViewModel(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }
}
```

`TestViewModel` represents the top level view model containing a collection of `PersonViewModel` objects in its property `People` of `ObservableCollection<PersonViewModel>` type:

```csharp
public class TestViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler? PropertyChanged;

    // fires notification if a property changes
    private void OnPropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }

    // collection of PersonViewModel objects
    public ObservableCollection<PersonViewModel> People { get; } =
        new ObservableCollection<PersonViewModel>();

    // number of people
    public int NumberOfPeople => People.Count;

    public TestViewModel()
    {
        People.CollectionChanged += People_CollectionChanged;

        People.Add(new PersonViewModel("Joe", "Doe"));
        People.Add(new PersonViewModel("Jane", "Dane"));
        People.Add(new PersonViewModel("John", "Dawn"));
    }

    // whenever collection changes, fire notification for possible updates
    // of NumberOfPeople and CanRemoveLast properties.
    private void People_CollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        OnPropertyChanged(nameof(NumberOfPeople));
        OnPropertyChanged(nameof(CanRemoveLast));
    }

    // can remove last item only if collection has some items in it
    public bool CanRemoveLast => NumberOfPeople > 0;

    // remove last item of the collection
    public void RemoveLast()
    {
        People.RemoveAt(NumberOfPeople - 1);
    }
}
```

It is populated with some 3 people's name it its constructor:

```csharp
public TestViewModel()
{
    People.CollectionChanged += People_CollectionChanged;

    People.Add(new PersonViewModel("Joe", "Doe"));
    People.Add(new PersonViewModel("Jane", "Dane"));
    People.Add(new PersonViewModel("John", "Dawn"));
}
```

Property `NumberOfPeople` contains the current number of items in collection `People` and property `CanRemoveLast` specifies if there are any items in the collection:

```csharp
// number of people
public int NumberOfPeople => People.Count;

...

// can remove last item only if collection has some items in it
public bool CanRemoveLast => NumberOfPeople > 0;
```

Every time the collection `People` changes we notify the binding that these two properties might have been updated:

```csharp
public TestViewModel()
{
    People.CollectionChanged += People_CollectionChanged;
    ...
}

// whenever collection changes, fire notification for possible updates
// of NumberOfPeople and CanRemoveLast properties.
private void People_CollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
{
    OnPropertyChanged(nameof(NumberOfPeople));
    OnPropertyChanged(nameof(CanRemoveLast));
}
```

There is a `RemoveLast()` method for removing the last item in the `People` collection:

```csharp
// remove last item of the collection
public void RemoveLast()
{
    People.RemoveAt(NumberOfPeople - 1);
}
```

MainWindow.axaml file contains all the XAML code for displaying the application:

```markup
<Window x:Name="TheWindow"
        xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="NP.Demos.ItemsPresenterSample.MainWindow"
        xmlns:local="clr-namespace:NP.Demos.ItemsPresenterSample"
        ...>
  <Window.Resources>
    <local:TestViewModel x:Key="TheViewModel"/>

    <DataTemplate x:Key="PersonDataTemplate">
      <Grid RowDefinitions="Auto, Auto"
            Margin="10">
        <TextBlock Text="{Binding Path=FirstName, StringFormat='FirstName: {0}'}"/>
        <TextBlock Text="{Binding Path=LastName, StringFormat='LastName: {0}'}"
                   Grid.Row="1"/>
      </Grid>
    </DataTemplate>

    <DataTemplate x:Key="TestViewModelDataTemplate">
      <Grid RowDefinitions="*, Auto, Auto">
        <ItemsPresenter Items="{Binding Path=People}"
                        ItemTemplate="{StaticResource PersonDataTemplate}">
          <ItemsPresenter.ItemsPanel>
            <ItemsPanelTemplate>
              <WrapPanel Orientation="Horizontal"/>
            </ItemsPanelTemplate>
          </ItemsPresenter.ItemsPanel>
        </ItemsPresenter>
        <TextBlock Text="{Binding Path=NumberOfPeople, StringFormat='Number of People: {0}'}"
                   Grid.Row="1"
                   HorizontalAlignment="Left"
                   Margin="10"/>
        <Button Content="Remove Last"
                IsEnabled="{Binding Path=CanRemoveLast}"
                Command="{Binding Path=RemoveLast}"
                Grid.Row="2"
                HorizontalAlignment="Right"
                Margin="10"/>
      </Grid>
    </DataTemplate>
  </Window.Resources>
  <ContentPresenter Content="{StaticResource TheViewModel}"
                    ContentTemplate="{StaticResource TestViewModelDataTemplate}"
                    Margin="10"/>
</Window>
```

The view model instance is defined at the top of the Window's resources section:

```markup
<local:TestViewModel x:Key="TheViewModel"/>
```

There are two data templates defined as Window's XAML resources: 

1. TestViewModelDatatemplate - the data template for the whole application. It is built around `TestViewModel` class and it uses PersonDataTemplate to display visuals corresponding to each person.
2. PersonDataTemplate - displays First and Last names of a single `PersonViewModel` item.

PersonDataTemplate is very simple - just two `TextBlocks` for first and last names - one on top of the other:

```markup
<DataTemplate x:Key="PersonDataTemplate">
  <Grid RowDefinitions="Auto, Auto"
        Margin="10">
    <TextBlock Text="{Binding Path=FirstName, StringFormat='FirstName: {0}'}"/>
    <TextBlock Text="{Binding Path=LastName, StringFormat='LastName: {0}'}"
               Grid.Row="1"/>
  </Grid>
</DataTemplate>
```

TestViewModelDataTemplate contains the `ItemsPresenter` \(for whose sake the sample was built\):

```markup
<ItemsPresenter Items="{Binding Path=People}"
                ItemTemplate="{StaticResource PersonDataTemplate}">
  <ItemsPresenter.ItemsPanel>
    <ItemsPanelTemplate>
      <WrapPanel Orientation="Horizontal"/>
    </ItemsPanelTemplate>
  </ItemsPresenter.ItemsPanel>
</ItemsPresenter>
```

Its `Items` property is bound to `People` collection of the `TestViewModel` class and its `ItemTemplate` property is set to be the PersonDataTemplate.

Its `ItemsPanel` is set to horizontally oriented `WrapPanel` just to demonstrate that we can change the way Visual items are arranged within the `ItemsPresenter` \(by default they'd be arranged vertically\).

TestViewModelDataTemplate has a `TextBlock` to show the total number of people within `People` collection:

```markup
<TextBlock Text="{Binding Path=NumberOfPeople, StringFormat='Number of People: {0}'}"
            Grid.Row="1"
            HorizontalAlignment="Left"
            Margin="10"/>
```

It also contains the button for removing last item. Button's command is bound to the `RemoveLast()` method of the view model and its `IsEnabled` property is bound to `CanRemoveLast` property of the view model:

```markup
<Button Content="Remove Last"
        IsEnabled="{Binding Path=CanRemoveLast}"
        Command="{Binding Path=RemoveLast}"
        Grid.Row="2"
        HorizontalAlignment="Right"
        Margin="10"/>
```

Finally we put the View Model instance and the DataTemplate together by using `ContentPresenter`: 

```markup
<ContentPresenter Content="{StaticResource TheViewModel}"
                  ContentTemplate="{StaticResource TestViewModelDataTemplate}"
                  Margin="10"/>
```

## Styles

## Animations

## 





