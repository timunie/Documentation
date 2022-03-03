# Creating Data Templates in Code

Avalonia also supports creating data templates in code with the [`FuncDataTemplate<T>`](http://reference.avaloniaui.net/api/Avalonia.Controls.Templates/FuncDataTemplate_1/) class.

At its simplest you can create a data template by passing a lambda which accepts an instance to the `FuncDataTemplate<T>` constructor:

```csharp
// The constructor requires two parameters. The first one is a bool-value which indicates if the object matches the DataTemplate. 
// The second parameter is a lambda which creates the actual control to display.
var template = new FuncDataTemplate<Student?>(
    student => student != null, // We require the student to be not null. You can also do other checks here.
    student => new TextBlock    // Our DataTemplate should be a simple TextBlock which shows the first name of the student.
    {
        [!TextBlock.TextProperty] = new Binding("FirstName"),
    });
```

Is equivalent to:

```markup
<DataTemplate DataType="{x:Type local:Student}">
    <TextBlock Text="{Binding FirstName}"/>
</DataTemplate>
```

