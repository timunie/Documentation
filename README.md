# 👋 Welcome

Welcome  to Avalonia Documentation, a steadily growing collection of guides, documentation and API reference for Avalonia UI.

If you can't find what you need here, [visit our friendly community](https://gitter.im/AvaloniaUI/Avalonia) where you'll find lots of people happy to help you, or if you're in need of more support you can opt for a [paid support plan](https://avaloniaui.net/support.html) which helps support the continued growth of the project.

{% hint style="info" %}
If you're familiar with WPF or UWP then you should feel right at home with Avalonia. Although Avalonia is not API compatible with either of these frameworks \(and so controls can't be used without porting\), there's a lot of similarity. If you're a WPF user we have a [page which describes the main differences between WPF and Avalonia](misc/wpf/).
{% endhint %}

## Runtime Requirements

### Windows

* .NET Core 3.1+ for Windows 8 and above.
* Windows 7 and below are not officially supported.

### Linux

* .NET Core 3.1+
* Debian 9 \(Stretch\)+
* Ubuntu 16.04+
* Fedora 30+

{% hint style="info" %}
Skia is built against glibc. If your distro uses something else instead, you need to build your own libSkiaSharp.so at [SkiaSharp](https://github.com/mono/SkiaSharp). We provide a precompiled binary _only_ for Intel x86-64. ARM/ARM64 support is planned.
{% endhint %}

### macOS

* .NET Core 3.1+
* macOS High Sierra 10.13+

## Getting Started

Everything you'll need to kick start your development with Avalonia.

{% page-ref page="docs/getting-started/ide-support/" %}

{% page-ref page="docs/getting-started/programming-with-avalonia/" %}

{% page-ref page="misc/wpf/" %}

## API Reference

A detailed reference to all the APIs available in Avalonia.

## Tutorials

Guided learning from the creators of Avalonia.

{% page-ref page="tutorials/todo-list-app/" %}

{% page-ref page="tutorials/music-store-app/" %}

## Samples

You can find our [samples](https://github.com/AvaloniaUI/Avalonia/tree/master/samples) on GitHub but there also exists a huge array of OSS projects built with Avalonia which provide valuable opportunities to learn. You can find a few of these projects on the [Awesome-Avalonia](https://github.com/AvaloniaCommunity/awesome-avalonia) list over on GitHub.

