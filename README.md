# 👋 Introduction to Avalonia

## Welcome

Welcome to Avalonia - the best open source, easy to learn .NET C\# multiplatform package for desktop \(Windows, Mac and Linux\), tablets and phone C\# development. Avalonia will soon also be working within a browser using WebAssembly technology.

After Microsoft discontinued Silverlight \(and I still think it was not the best decision\), the only popular multiplatform frameworks available for building UI applications  remained Web solutions \(JavaScript/TypeScript\), Xamarin and Java. Unfortunately all those packages have a lot of shortcomings which will be detailed in below.

## Why Avalonia is a Hit

* Avalonia allows creating applications that look and behave the same on most popular Desktop and Mobile platforms \(Windows, Mac, various flavors of Linux, iOS and Android\) while also allowing the platform specific customizations.
* Avalonia is supported by the following large companies:

  * [![](.gitbook/assets/se.png)](https://www.se.com/us/en/)
  * [![](.gitbook/assets/jblogo%20%282%29.png)](https://www.jetbrains.com/)
  * [![](.gitbook/assets/unitylogo.png)](https://unity.com/)
  * [![](.gitbook/assets/logo_thales_500.png)](https://www.thalesgroup.com/)
  * [![](.gitbook/assets/synergy-logo.png)](https://synergysports.com/)
  * [![](.gitbook/assets/gritworld-logo.png)](https://en.gritworld.com/)
  * [![](.gitbook/assets/wasabi-wallet-logo.svg)](https://www.wasabiwallet.io/)
  * [![](.gitbook/assets/mailbird-logo.png)](https://www.getmailbird.com/)

  and many other smaller companies.

* Several extremely popular .NET open source projects have been ported to Avalonia. Among them are
  * [AvalonEdit](http://avalonedit.net/) - the port is available as [AvaloniaEdit](https://github.com/AvaloniaUI/AvaloniaEdit) - Rich Text Editor.
  * [RoslynPad](https://roslynpad.net/) - C\# code editor with intellisense.
* Avalonia \(just like WPF and Silverlight\) is fully compositional: you can create a button out of Avalonia primitives in a fashion similar to creating a complex page, while its competitors \(Web, Xamarin and Java\) packages are not compositional to the same degree as will be detailed later.
* Avalonia UI, unlike JavaScript has full advantage of a strongly typed language. Also Avalonia has full advantage of a compiled-to-binary code language and is in general considerably faster than JavaScript or TypeScript.
* Avalonia is an open source multiplatform descendant of great Microsoft packages - WPF and Silverlight which brought the UI development on a completely new level creating a set of new concepts and which allow an experienced WPF developer create a full blown UI application in fraction of time and effort in comparison to other technologies. Here is the list of some of the new WPF concepts:

  * Visual and Logical Trees
  * Attached or Dependency Properties which can be defined outside of the object on which they are used and do not take any extra memory unless they are assigned a non-default value and have a special event to fire when their values change.
  * Attached Routed Events that can be defined outside of the objects that fire them and can propagate and be handled up and down on the trees.
  * Bindings and the Related MVVM pattern
  * ControlTemplates
  * DataTemplates
  * Styles
  * Behaviors - way to modify and augment the behavior of a C\# class without modifying the class itself by using the events.

  All these paradigms have been implemented for multiple platforms in Avalonia - many on them in a more powerful and better way than they are in WPF.

* Avalonia is being developed very fast by [extremely bright people](https://github.com/AvaloniaUI/Avalonia/graphs/contributors) and even in its current state it is already considerably more powerful and less quirky than WPF in many respects. But what is coming is going to be even better.
* Avalonia is [100% open source - and will continue as an open source project](https://github.com/AvaloniaUI/Avalonia) and has great and constantly expanding documentation and a great number of open source samples showing how to use Avalonia.
* Avalonia has a decent [free support on Avalonia Gitter](https://gitter.im/AvaloniaUI/Avalonia), one can also post questions on [Avalonia Github Discussions](https://github.com/AvaloniaUI/Avalonia/discussions). Some [business support is also available](https://avaloniaui.net/support.html).
* Avalonia has Visual Studio and Rider support for creating XAML files while having advantage of XAML intellisense. Visual Studio also has a designer view available.
* Avalonia has a tool that facilitate the visual development and debugging \(akin to WPF Snoop\). This tool is also multiplatform - meaning you can use it on Mac and Linux and completely open source.

## Why Avalonia is Better than other Multiplatform UI Frameworks

Here we shall go over the reasons why Avalonia is better for Desktop/Mobile development than other frameworks including Web development \(JavaScript/TypeScript\), Xamarin, Java and C++. 

The main disadvantage of every other competing platform is that it is not compositional to the same degree as Avalonia \(or WPF\). In JavaScript/TypeScript, Xamarin or Java - the basic primitives are buttons, dropdowns, menus and so on, while in Avalonia, the buttons, dropdowns, menus, check boxes can be composed out of much more basic primitives. Because of that the customization of look and feel and control behaviors in Avalonia is much easier to achieve.

For example, you cannot pack HTML5 canvas rectangles into a reusable button control that will show in similar ways in the center of the Web page and at the bottom of it \(without some extra customization\). Instead, the controls come predefined with the browser with a million ways to customize them \(some of the customizations are specific to each browser\). In Web development - creating a re-usable custom control is virtually impossible - you are stuck with customization of already existing controls or some widgets coming from the 3rd party tools.

Web programming \(based on JavaScript/TypeScript languages and  frameworks like Angular or React\) is not very suitable for building desktop and mobile applications - I saw a couple of projects failing because the clients were expecting to have as high degree of customization as they had in a WPF project. There is a very successful state of art Visual Studio Code application built in TypeScript by Microsoft - I would warn you that a company that is less well funded than Microsoft, will not achieve results that are even anywhere close to Visual Studio Code within reasonable time. Using Avalonia, though, it is quite reasonable to build a comparable application within several months to a year. Besides, even Visual Studio Code does not allow to have multi-window layouts, while Visual Studio \(built mostly in WPF\) allows decomposing the application into multiple windows by dragging the panes out of the main application - for some people it is a big deal.

Also JavaScript \(and TypeScript\) are considerably slower than C\#: you can check [benchmarks ](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/csharp.html)website where C\# and JavaScript on Node.js are matched against Java language with C\# being faster and JavaScript slower on average than Java. This is to be expected since C\# is compiled to pseudo code  which is much closer to the native code than the Web languages.

Xamarin \(and its descendant MAUI\) is also not as compositional as Avalonia \(or WPF\). Its primitives are also basic controls like Buttons, Checkboxes, menus etc. In fact those controls are displayed and behave differently on each of the OS - since they are based on the native OS controls. Also Xamarin \(and MAUI\) were originally built for the mobile development and are not very suited for desktop development. 

People who are serious about the look and feel of their UI application usually do not use Java language to build it. Java applications are knows for very old look and feel and few if any customization capabilities.

There are also some multiplatform C++ WPF-Like frameworks which are not well known to mention their names, but they force the developers to work in C++ which is plagued by the common C++ development problem - very slow compilation/linking and prototyping difficulty because of that. 

Also programming Xamarin or C++ will force to compile the project into separate binaries, while .NET Core programming with Avalonia will allow to keep the binaries the same while testing the product on different machines.

Another reason to prefer Avalonia is that it implements all the great concepts that WPF originally came up with and which make it considerably easier to build UI applications, while the competing frameworks at most implement just a pale imitation of those concepts. E.g. React and Angular frameworks implement bindings, but do not implement bindings that can connect to a property defined up the Visual Tree. Custom attached properties and custom Routed Events are not implemented either. Xamarin does not support Routed Events that can travel on trees, neither it supports a differentiation between the logical and visual trees.  

## Supported .NET Version

Avalonia is supported for .NET Core 3.1 and higher.

## Supported Platforms

Avalonia is supported on the following platforms:

* Windows 8 and higher
* MaxOS High Sierra 10.13 and higher
* for Linux:
  * Debian 9 \(Stretch\) and higher
  * Ubuntu 16.5 and higher
  * Fedora 30 and higher

## Supported Development Environments

The following environments support Avalonia XAML with intellisense:

* Visual Studio 2017 and higher \(with or without Resharper 2020.3\). Avalonia Visual Designer is also supported.
* JetBrains Rider 2020.3 and higher \(Avalonia Designer coming soon to Rider\).

## Installing Avalonia Extension for Visual Studio

In order to start using Avalonia, you need to set up your development environment \(Visual Studio or Rider\) first. 

You achieve in in Visual Studio by installing "Avalonia for Visual Studio" extension as described below.

Open you VS extension manager \(in VS 2019 it is located under Extensions menu\). Find the extension called "Avalonia for Visual Studio" online and install it:

![Installing Avalonia for Visual Studio extension](.gitbook/assets/installavaloniaextension.png)

Avalonia for Visual Studio extension installs the templates for creating the Avalonia UI projects and various XAML based files and it also installs the intellisense for XAML. 

## Creating and Running a Simple Avalonia Application under Visual Studio

Once you have installed the "Avalonia for Visual Studio" extension, start Visual Studio choose New-&gt;Project menu item for the File menu:

![Creating new Project in Visual Studio](.gitbook/assets/createnewproject.png)

and choose "Avalonia Application" project type:

![Choosing Avalonia Application Project Type](.gitbook/assets/avaloniaappprojecttype.png)

 Press button "Next" and choose the name and location for your first Avalonia project, e.g:

![Creating simple avalonia project](.gitbook/assets/createsimpleavaloniaproject.png)

Pressing button "Create" will create the Avalonia Application project. It will have dependencies on three Avalonia nuget packages:  `Avalonia`, `Avalonia.Desktop` and `Avalonia.Diagnostics`:

![Avalonia Application project Files and Dependencies](.gitbook/assets/projectpackagesandfiles.png)

The newly created project contains five files:  _App.axaml_, _App.axaml.cs_, _MainWindow.axaml_, _MainWindow.axaml.cs_ and _Program.cs_. Note that Avalonia XAML files have "._axaml_" extension \(unlike WPF XAML files that have "._xaml_" extension\). The Avalonia XAML syntax is very similar to WPF XAML syntax, with some specialty when it comes to the so called Style Selectors \(which will be explained in the future\). 

Among the five files listed above, it is likely you will have to modify _MainWindow.axaml_ and _MainWindow.axaml.cs_ files most, then perhaps slightly change also _App.axaml_ and _App.axaml.cs_ files and you probably won't have to change _Program.cs_ file ever.

 Open _MainWindow.xaml_ file and replace its content \(which by default consists of "`Welcome to Avalonia!`" text\) with the following code:

```markup
<Button x:Name="CloseWindowButton"
        Content="Close Window"
        HorizontalAlignment="Center"
        VerticalAlignment="Center"
        Padding="10,5"/>
```

If you run the application now, there will be a Button in the middle of the window \(which is ensured by Horizontal and Vertical Alignments - the words "**Close Window**" will be written in the middle of the button \(`Button`'s content\) and the margins from the "**Close Button**" text to the sides of the button will be 10 generic pixels on the right and left and 5 generic pixels at the top and the bottom \(specified by the `Padding` property\):

![](.gitbook/assets/windowwithbutton.png)

 So far so good, but if you press the button, nothing is going to happen. We need to try to connect the button's "Click" event to the action that closes the window. In this first sample, we are going to employ the simplest, but also the **worst** way of achieving such purpose - the code behind. File _MainWindow.xaml.cs_ contains the so called "code behind" - C\# code for the _MainWindow.xaml_ file:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
#if DEBUG
        this.AttachDevTools();
#endif
    }

    private void InitializeComponent()
    {
        AvaloniaXamlLoader.Load(this);
    }
}
```

 We named out button "`CloseWindowButton`". In WPF, the corresponding class member would have been generated. `Avalonia`, still does not include this feature, but we can easily find the button by adding the following line:

```csharp
var button = this.FindControl<Button>("CloseWindowButton");
```

after `InitializeComponent();` is called within the constructor. Then we can add a handler to the button's `Click` event: `button.Click += Button_Click;`. Finally, within the handler we can call `Close()` method on the window:

```csharp
private void Button_Click(object? sender, RoutedEventArgs e)
{
    this.Close();
}
```

 The full code for `MainWindow` constructor and the handler will look like below:

```csharp
public MainWindow()
{
    InitializeComponent();
#if DEBUG
    this.AttachDevTools();
#endif
    var button = this.FindControl<Button>("CloseWindowButton");

    button.Click += Button_Click;
}

private void Button_Click(object? sender, RoutedEventArgs e)
{
    this.Close();
}
```

Now if you run the application and press "**Close**" button, the window will close.

The code for this application is located under [_NP.Demos.SimpleAvaloniaProject_](https://github.com/npolyak/NP.CodeForAvaloniaInEasySampleArticle/tree/main/NP.Demos.SimpleAvaloniaProject), but if you are new to Avalonia and WPF, it is imperative that you should go over the exercise above.

