# Avalonia Building Blocks Explained with Samples

## Introduction to Avalonia Building Blocks

![](../.gitbook/assets/buildingblocks.jpg)

The best way to introduce a development framework is to show its off the shelf capabilities first in the most straightforward manner and then, based on that foundation explain the new concepts that come together with the framework. 

The Avalonia building blocks \(and the corresponding samples\) will provide the engineers with the breadth of knowledge of how to operate with Avalonia, while the explanation of the concepts will cover the knowledge depth.

Here and in the rest of the Avalonia UI guides, we do not require prior knowledge of WPF, but it will be good to go over the previous guide - [Introduction to Avalonia](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2) and especially its [Creating and Running a Simple Avalonia Application under Visual Studio sample](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2) section.

The content of this guide is also covered in Multiplatform UI Coding with [AvaloniaUI in Easy Samples. Part 1 - AvaloniaUI Building Blocks](https://www.codeproject.com/Articles/5308645/Multiplatform-UI-Coding-with-AvaloniaUI-in-Easy-Sa) article and all the code samples are located under [NP.CodeForAvaloniaInEasySampleArticle](https://app.gitbook.com/@avalonia-ui/s/avalonia-docs-2/#creating-and-running-a-simple-avalonia-application-under-visual-studio) repository.

The following topics are covered in this article:

* The most useful Built-In Controls
* The primitives \(controls that serve as the primitive building blocks for the composition\)
* Panels
* Brushes
* Transform

## How to Read this Guide

This guide intends to be a hands on Avalonia tutorial highlighting Avalonia basic capabilities. It is full of coding examples that I recommend to build and run on your computer. It will be even better if you try to create and run examples similar to those of this guide yourself.

## Information about the Samples

All the code samples are located under [NP.CodeForAvaloniaInEasySampleArticle](https://github.com/npolyak/NP.CodeForAvaloniaInEasySampleArticle) repository.

All of the samples in this guide were created using Visual Studio 2019, .NET 5.0 and correspondingly C\# 9.0 though they can be easily downgraded to previous versions of .NET and C\#.

All the samples should be running on every one of the supported desktop platforms: Window, Linux and MacOS.

## Avalonia Built-In Controls

### Introduction to Avalonia Controls

In this section, I am going to describe some of the Built-in Avalonia controls which are most helpful for building an application. Note that for the sake of brevity, I am not going to describe every built-in control, only those that are most commonly used.

If you want to learn about the rest of the built in controls, you can:

* Download or clone the Avalinia source code from [Avalonia Source Code on Github](https://github.com/AvaloniaUI/Avalonia).
* Use Visual Studio to open _Avalonia.sln_ solution from the top level folder.
* Navigate within the solution explorer to `ControlCatalog.Desktop` project under _Samples_ Visual Studio folder.
* Make `ControlCatalog.Desktop` your startup project, then build and run it.

 The Windows application containing most \(if not all\) of the built-in controls will popup and you'll be able to see the capabilities of various control. By tracking them in XAML code under _Samples/Pages/_ Visual Studio folder, you will also be able to see how to create and change properties of those controls.

 New control creation and customization will be discussed in future sections as well as the most useful controls `ContentPresenter` and `ItemsPresenter` that can give a visual representation to a non-visual object or a collection of non-visual objects correspondingly.

The purpose of this section is to give an overview of the Built-In Avalonia controls, not a detailed description of their capabilities.

 Note that `Control` class in Avalonia is more primitive than the same named class in WPF - Avalonia `Control` does not have a template \(explanations on was Templates are and how to use them will be given in future sections\). The controls that have templates inherit from `TemplatedControl` class which implements `ITemplatedControl` interface. Because of that Avalonia `Image`, `Shape` and`Panel` classes are derived from the `Control`, not from `Visual` class \(as they are in WPF\).

### Built-In Controls Project

In this subsection, we'll describe some of the most useful controls in WPF sense, i.e., templated controls, not more primitive images, shapes and panels.

The code for these subsection is located under _NP.Demos.BuiltInControls.sln_ solution.

Here is what you are going to see if you build and run the solution:

![Built-In Controls](../.gitbook/assets/builtincontrols.png)

 All the code specific to these controls is located within _MainWindow.xaml_ files. Let us go over each one of the controls and describe the XAML code related to them.

#### **TextBlock**

`TextBlock` is the only control from among the controls describes in the section, that cannot be retemplated - it is derived from `Control` \(not from `TemplatedControl` class\). So it is more of a primitive, rather than a composite control in WPF sense, but placed here, because it is one of the most important building blocks and \(from my viewpoint\) should be described first.

`TextBlock` represents simple text. Its most important property is `Text` of C\# `string` type. `Text` contains the text you want to display as the `TextBlock`.

`TextBlock` has a lot of properties that allow text customization, including:

* `Foreground` - the color of the text
* `FontSize` - self descriptive
* `FontFamily` - specifies the name of the font
* `FontWeight` - usually switches between Normal and Bold - for bold text
* `TextWrapping` - specifies if the text should be wrapped to multiple lines
* `TextTrimming` - specifies if ellipses \('...'\) characters should be shown if part of the text is invisible - the text stretches beyond the size of the control

Many of the properties listed above are also applicable to other controls that might have text, e.g., buttons, menus, ListBoxes and so on.

The simple XAML code to create a text block is e.g.:

```markup
<TextBlock Text="Hello World!">
```

#### **TextBox**

Here is an image for the `TextBox` sample:

![Image 6](https://www.codeproject.com/KB/Articles/5308645/TextBoxSample.png)

If you type anything within the `TextBox`, its property `Text` will get updated with the typed text.

I used binding to duplicate the text typed within the `TextBox` by the `TextBlock` placed underneath. Here is the XAML code:

```markup
<Grid Grid.Row="1">
    <Grid.RowDefinitions>
      <RowDefinition Height="Auto"/>
      <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBox x:Name="TheTextBox"
             Width="150"
             Height="27"
             HorizontalAlignment="Left"
             Grid.Row="0"/>
    <TextBlock Text="{Binding Path=Text, ElementName=TheTextBox}"
               Grid.Row="1"
               Height="17"/>
  </Grid>
</Grid>
```

We defined the `Grid` panel with two rows - text box is in the top row \(`Grid.Row="0"`\) and `TextBlock` is in the second row \(`Grid.Row="1"`\).

We bind the `Text` property of the `TextBlock` to the `Text` property of the `TextBox` \(named "`TheTextBox`" via its `x:Name` attribute\) by using `ElementName` binding: `Text="{Binding Path=Text, ElementName=TheTextBox}"`. This produces the effect of the `TextBlock`below repeating the text entered into the `TextBox`.

#### **Button**

Next control we'll describe is `Button`:

![Button](https://www.codeproject.com/KB/Articles/5308645/ButtonSample.png)

Here is the code for our button:

```markup
<Button Content="Button"
        Padding="10,5"
        Grid.Row="1"/>
```

`Padding="10,5"` above means that the button extends from its content 10 generic pixels left and right and 5 top and bottom. `Grid.Row="1"` means that the button is placed in the second row of the `Grid` \(the first row is occupied by the header\).

Buttons define `Click` routed event - which is fired when the button is clicked. There are three ways to call C\# code on a Button click:

* Using code behind - as was shown in the `NP.Demos.SimpleAvaloniaProject` sample \(the **worst** method to use\).
* Using its `Command` property which can be bound to a View Model property. The View Model property in turn can define the lambda expression to call, when the button is clicked and also the property to control whether the button is enabled or not. This will be describe in detail in future articles.
* Using a behavior listening to `Click` routed event and calling a C\# method when the event is fired. \(this will also be described later\) - this is the best method.

#### **ListBox**

![Image 8](https://www.codeproject.com/KB/Articles/5308645/ListBoxSample.png)

`ListBox` displays a collection of items with capability of selecting one item at a time. If the number of items exceeds the size of the `ListBox`, it will display the scroll bars.

The best way to use the list box is by binding its `Items` property to a collection. How to do it will be shown below. In our case, we simply created `ListBoxItem`s within the XAML code to populate it:XMLCopy Code

```text
<ListBox x:Name="TheListBox">
  <ListBoxItem Content="Item 1"/>
  <ListBoxItem Content="Item 2"/>
  <ListBoxItem Content="Item 3"/>
  <ListBoxItem Content="Item 4"/>
</ListBox>
```

The most important properties of the `ListBox` are `Items` mentioned above and selection related properties: `SelectedIndex` and `SelectedItem`. The `SelectedIndex` of this `ListBox` is bound to the `SelectedIndex` of the `ComboBox` described in the next subsection, so when you change the selected item on each of them, the other will react in the same fashion.

**ComboBox**

![Image 9](https://www.codeproject.com/KB/Articles/5308645/ComboBoxSample.png)

`ComboBox` is also called `DropDownBox` in various other frameworks. Just like `ListBox` it also stores a collection of items, but only selected item is displayed all the time - the other items are only displayed in a popup \(or rather drop-down\) when the mouse pointer clicks the arrow on its right hand side. The image above displays the `ComboBox` without a selected item but with the open `dropdown`. Here is the code that I used to create and populate the `ComboBox`:XMLCopy Code

```text
<ComboBox VerticalAlignment="Top"
          Grid.Row="1"
          SelectedIndex="{Binding Path=SelectedIndex, ElementName=TheListBox}">
  <ComboBoxItem Content="Item 1"/>
  <ComboBoxItem Content="Item 2"/>
  <ComboBoxItem Content="Item 3"/>
  <ComboBoxItem Content="Item 4"/>
</ComboBox>
```

Same as in case of `ListBox`, `ComboBox`es' main properties are `Items` \(to be bound to a collection\), `SelectedIndex` and `SelectedItem`. The example shows how to bind the `SelectedIndex` of the `ComboBox` to that of the `ListBox` to the left of it: `SelectedIndex="{Binding Path=SelectedIndex, ElementName=TheListBox, Mode=TwoWay}"`, so that when one changes Selection, the other will also change.

**ToggleButton**

![Image 10](https://www.codeproject.com/KB/Articles/5308645/ToggleButtonSample.png)

`ToggleButton` is a control that has two states - Checked and Unchecked controlled by its boolean property `IsChecked`. Every time the button is clicked, its `IsChecked` property toggles its value from `false` to `true` and vice versa. The button's background changes depending on whether it is checked on unchecked.XMLCopy Code

```text
<ToggleButton x:Name="TheToggleButton" 
              Content="Toggle Button"/>
```

Again, in order to demonstrate the power of the binding, I bound the `ToggleButton`'s `IsChecked` property to that of the `CheckBox` next to it so that they change in sync.

**CheckBox**

![Image 11](https://www.codeproject.com/KB/Articles/5308645/CheckBoxSample.png)

`CheckBox` is very similar to the toggle button, but looks different \(as you can see\). Here is the XAML code for `CheckBox`:XMLCopy Code

```text
<CheckBox Content="Check Box"
          VerticalAlignment="Top"
          Grid.Row="1"
          IsChecked="{Binding Path=IsChecked, ElementName=TheToggleButton}"/>
```

You can see the binding that connects its `IsChecked` property to that of the `ToggleButton` on its left.

`CheckBox` also has a switch property `IsThreeState` and when it is set to `true`, the `CheckBox` can switch between three states - false, true and undefined - which corresponds to its `IsChecked` property set to `null`:

![Image 12](https://www.codeproject.com/KB/Articles/5308645/ThreeStateCheckBoxSample.png)

Here is the code for three state `CheckBox`:XMLCopy Code

```text
<CheckBox Content="Three State Check Box"
          IsThreeState="True"/>
```

**ContextMenu**

![Image 13](https://www.codeproject.com/KB/Articles/5308645/ContextMenuSample.png)

`ContextMenu` opens up when right mouse button is clicked on some area or control. Here is the code:XMLCopy Code

```text
<Grid Grid.Row="1"
    Background="Transparent">
<Grid.ContextMenu>
  <ContextMenu Grid.Row="1">
    <MenuItem Header="Item1">
      <MenuItem Header="SubItem1"/>
      <MenuItem Header="SubItem2"/>
    </MenuItem>
    <MenuItem Header="Item2"/>
    <MenuItem Header="Item3"/>
    <MenuItem Header="Item4"/>
  </ContextMenu>
</Grid.ContextMenu>
<TextBlock Text="Right Click To Open Context Menu"
           VerticalAlignment="Center"/>
</Grid>
```

**Menu**

![Image 14](https://www.codeproject.com/KB/Articles/5308645/MenuSample.png)

Menus are usually placed at the top of the window, but can also appear in other places. Here is the XAML code for the `Menu` sample:XMLCopy Code

```text
<Menu Grid.Row="1">
    <MenuItem Header="FILE">
      <MenuItem Header="New"/>
      <MenuItem Header="Open"/>
      <MenuItem Header="Save"/>
    </MenuItem>
    <MenuItem Header="EDIT">
      <MenuItem Header="Copy"/>
      <MenuItem Header="Paste"/>
    </MenuItem>
</Menu>
```

**Popup**

Popup is a control that opens a light weight window next to the so called popup's `PlacementTarget`:

![Image 15](https://www.codeproject.com/KB/Articles/5308645/PopupWindowSample.png)

Whether the popup is open or not is controled \(and reflected\) by its `IsOpen` property which in our case is tied to the `IsChecked` property of the `ToggleButton` that controls \(and reflects\) the popup's state:XMLCopy Code

```text
<Grid Grid.Row="1">
    <Grid.RowDefinitions>
      <RowDefinition Height="Auto"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <ToggleButton x:Name="OpenClosePopupButton"
                  Content="Open/Close Popup"/>

    <Popup x:Name="ThePopup"
           Grid.Row="1"
           IsOpen="{Binding Path=IsChecked, ElementName=OpenClosePopupButton, Mode=TwoWay}"
           StaysOpen="False"
           PlacementMode="Bottom"
           PlacementTarget="{Binding ElementName=OpenClosePopupButton}">
      <Grid x:Name="PopupsContent"
            Background="Red"
            Width="150"
            Height="70">
        <TextBlock Text="Popup's Content"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
      </Grid>
    </Popup>
</Grid>
```

We use a two way binding to bind the Popup's `IsOpen` property to the `ToggleButton`'s `IsChecked` property so that changing each one of them would affect the other.

`StaysOpen` property is set to `false` - meaning that clicking out of the popup area will close the popup.

`PlacementTarget` property specifies an element with respect to which the `Popup` is going to be positioned.

`PlacementMode="Bottom"` means that the `Popup` will be positioned at the bottom of the placement target.

**Window**

![Image 16](https://www.codeproject.com/KB/Articles/5308645/WindowSample.png)

When you click the button of the "`Window`" sample, a small window will open.

Here is the XAML code for the window sample:XMLCopy Code

```text
<Button x:Name="OpenWindowButton"
        Content="Open Window"/>
```

The C\# code that opens the window is hooked via code-behind - not because it is a good way to do it \(actually it is the **worst** as was mentioned above\) - but because it is the simplest way and the easiest to understand. Here is the relevant C\# code from _MainWindow.axaml.cs_ file:C\#Copy Code

```text
public MainWindow()
{
    ...

    var openWindowButton = this.FindControl<Button>("OpenWindowButton");

    openWindowButton.Click += OpenWindowButton_Click;
    ...
}
private void OpenWindowButton_Click(object? sender, RoutedEventArgs e)
{
    // Create the window object
    Window sampleWindow = 
        new Window 
        { 
            Title = "Sample Window",
            Width = 200,
            Height = 200
        };

    // open the window
    sampleWindow.Show();
}
```

In `MainWindow`'s constructor \(after the call to `InitializeComponent()`\) we find the button by its name and attach a handler to its `Click` event. Within that handler, we create the new `Window` object and call `Show()` method on it to dispay \(open\) the window.

**Modal Window**

Modal Window is also called a Dialog - it is a window that prevents any actions on its ancestor windows until it is closed.

![Image 17](https://www.codeproject.com/KB/Articles/5308645/ModalWindowSample.png)

Pressing "**Open Modal \(Dialog\) Window**" button will open a dialog window which completely blocks the main window until it is closed. Here is the XAML code:XMLCopy Code

```text
<Button x:Name="OpenModalWindowButton"
        Content="Open Modal (Dialog) Window"/>
```

Again, we connect the C\# code using the no-good code-behind paradigm:C\#Copy Code

```text
public MainWindow()
{
    ...

    var openModalWindowButton = this.FindControl<Button>("OpenModalWindowButton");

    openModalWindowButton.Click += OpenModalWindowButton_Click;
}
...
private void OpenModalWindowButton_Click(object? sender, RoutedEventArgs e)
{
    // Create the window object
    Window sampleWindow = 
        new Window 
        { 
            Title = "Sample Modal (Dialog) Window",
            Width = 200,
            Height = 200
        };

    // open the modal (dialog) window
    sampleWindow.ShowDialog(this);
}
```

The only difference between this and previous samples, is that here we call `sampleWindow.ShowDialog(...)` method instead of `sampleWindow.Show()`, passing to it the current window as the parent of the dialog.

**ToolTip**

`ToolTip` is the temporary popup that opens next to the mouse pointer over an element that defines that `ToolTip`:

![Image 18](https://www.codeproject.com/KB/Articles/5308645/ToolTipSample.png)

We define the `ToolTip` in XAML as following:XMLCopy Code

```text
<Grid Height="40"
      Background="Aqua"
      ToolTip.Tip="This is the ToolTip">
```

Most of the time `ToolTip` is just a text \(as in our case\) but sometimes it can become more involved as will be described in the future.

**TabControl**

`TabControl` allows to display different tabs - each tab containing different content:

![Image 19](https://www.codeproject.com/KB/Articles/5308645/TabControlSample.png)

Switching between the tabs in our sample will change the displayed text from "`Hello World!`" to "`Hi World!`".

Here is the very simple XAML code to achieve that:XMLCopy Code

```text
<TabControl Grid.Row="1">
    <TabItem Header="Tab 1">
        <TextBlock Text="Hello World!"/>
    </TabItem>
    <TabItem Header="Tab2">
        <TextBlock Text="Hi World!"/>
    </TabItem>
</TabControl>
```



