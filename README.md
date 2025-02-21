<h1 align="center">zgt: a GUI library for Zig</h1>
<h5 align="center">GUIs in Zig, but idiomatic</h5>

**As of now, zgt is NOT ready for use in production as I'm still making breaking changes**

---

![the glorius software in action](https://raw.githubusercontent.com/zenith391/bottom-zig-gui/main/.github/screenshot.png) 

## Introduction

zgt is a **graphical user interface library for Zig**. It is mainly intended for creating applications using native controls from the operating system.
zgt is a declarative UI library aiming to be easy to write for and versatile.

It has been made with the goal to empower standalone UI applications, integration in games or any other rendering process is a non-goal.

## Usage

**zgt can be used as any other library** using the Zig build package system. It only requires adding this code to your `build.zig` file (it also manages backend-specific configuration):

```zig
try @import("zgt/build.zig").install(exe, "./path/to/zgt");
```

A simple application using zgt:

```zig
const zgt = @import("zgt");
const std = @import("std");

pub fn main() !void {
    try zgt.backend.init();

    var window = try zgt.Window.init();
    try window.set(
        zgt.Column(.{}, .{
            zgt.Row(.{}, .{
                zgt.Button(.{ .label = "Save", .onclick = buttonClicked }),
                zgt.Button(.{ .label = "Run",  .onclick = buttonClicked })
            }),
            // Expanded means the widget will take all the space it can
            // in the parent container
            zgt.Expanded(
                zgt.TextArea(.{ .text = "Hello World!" })
            )
        })
    );

    window.resize(800, 600);
    window.show();
    zgt.runEventLoop();
}

fn buttonClicked(button: *zgt.Button_Impl) !void {
    std.log.info("You clicked button with text {s}", .{button.getLabel()});
}
```
It is easy to add something like a button or a text area. The example can already be used to notice a widget's parameters are usually enclosed in anonymous
structs (`.{ .label = "Save" }`). You can also see that simply wrapping a widget with `zgt.Expanded( ... )` will tell it to take all the space it can.

