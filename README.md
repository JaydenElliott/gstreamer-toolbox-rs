# Gstreamer Toolbox - for Rust and C plugin development

**This repo is currently in progress**

A collection of resources I found useful when writing custom gstreamer plugins in Rust. Anyone deep enough in the gstreamer ecosystem knows how painful it can be to find up to date documentation or examples.

## Table of contents

1. [Start Here](#start-here)
2. [Primary Sources](#primary-sources)
3. [Writing Plugins]()
4. [Useful IDE config](#useful-ide-config)
5. [Nvidia Deepstream](#nvidia-deepstream)

<br>

## Start Here

TODO

- https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/main/tutorial

Prepare to read **a lot** of plugin implementations and examples. Although there is a fair bit of documentation, it won't answer a lot of questions you will have about specifics. My usual workflow for findings answers is:

<br>

# Primary Sources

> 95% of your questions can be answered by reading these links

<br>

## Gstreamer docs

<https://gstreamer.freedesktop.org/documentation/>

Useful links:

- [Basic concepts (pads, elements, pipelines etc...)](https://gstreamer.freedesktop.org/documentation/application-development/introduction/basics.html?gi-language=c)
- [Architecture and design overview](https://gstreamer.freedesktop.org/documentation/additional/design/overview.html?gi-language=c)
- [Tutorial (in c)](https://gstreamer.freedesktop.org/documentation/tutorials/index.html?gi-language=c) or [rust port](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs/-/tree/main/tutorials/src/bin)
- [API reference](https://gstreamer.freedesktop.org/documentation/libs.html?gi-language=c)
- [Plugin reference](https://gstreamer.freedesktop.org/documentation/plugins_doc.html?gi-language=c)

<br>

## CLI - gst-inspect-1.0

<https://gstreamer.freedesktop.org/documentation/tools/gst-inspect.html?gi-language=c>

Essentially the gstreamer plugin reference, however allows you to query your own custom plugins and quickly verify an elements:

- Description
- Pad capabilities
- Properties
- Signals

This will only work for plugins you have installed (elaborate on PKG_CONFIG_PATH and /usr/lib/x86_64-linux-gnu/pkgconfig), see [writing plugins](#writing-plugins).

<br>

## Rust bindings

[Gstreamer Rust Bindings](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs)

This repository, as well as [gst-plugins-rs](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs) are, imo the most useful references for writing your own plugins. @sdroege has done a momentous job building these bindings and writing excellent tutorials/documentation. I frequently visit the plugin examples and find the following resources extremely useful for newcomers and experienced gst devs alike:

- [Examples of gst api usage from rust](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs/-/tree/main/examples/src/bin)
- [Installing the bindings](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs#installation)
- [Gstreamer tutorial code rewritten in rust](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs/-/tree/main/tutorials/src/bin)
- [Rust docs](https://gstreamer.pages.freedesktop.org/gstreamer-rs/stable/latest/docs/gstreamer/)
  <br>

<br>

## Rust Plugins

https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs

Contains endless plugin examples, a great tutorial and is constantly active. Core maintainers are very reponsive if you have any questions not covered in their examples/docs.

- [Plugin examples](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs)
- [Fantastic tutorial on writing gst plugins in rust](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/main/tutorial)
- [Tracer examples](https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/main/utils)

<br>

# Observability

## Tracers

Tracers are extremely useful for monitoring pipeline throughput, element latency, cpu usage and a variety of other metrics.

- [Gstreamer tracing docs](https://gstreamer.freedesktop.org/documentation/additional/design/tracing.html?gi-language=c)
  - Not great but has some good [examples](https://gstreamer.freedesktop.org/documentation/additional/design/tracing.html?gi-language=c#try-it) of usage. These will work directly with a rust application.
- [GstShark Tracer Plugins](https://developer.ridgerun.com/wiki/index.php/GstShark_-_Tracers)

  - Set of plugins that extend/replace some of the core gstreamer ones. Very well documented.

- [todo - tracer graphs (not pipeline ones)](https://gstreamer.freedesktop.org/documentation/check/gstharness.html?gi-language=c)

- [todo - devtool tracer parser](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-devtools/tracer)
  - have been unable to get this working

## Dot Graph Generation

[Core c reference](https://gstreamer.freedesktop.org/documentation/gstreamer/debugutils.html?gi-language=c)

### From rust application

1. Define `GST_DEBUG_DUMP_DOT_DIR` environment variable

2. Upcast pipeline to `gst::Element` then call `element.debug_to_dot_file()`. Example usage from [gstreamer-rs playbin example](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs/-/blob/main/examples/src/bin/playbin.rs#L129):

```
let bin_ref = playbin.downcast_ref::<gst::Bin>().unwrap();
bin_ref.debug_to_dot_file(gst::DebugGraphDetails::all(), "PLAYING");

```

### Using GstShark

[Easy pipeline image generation](https://developer.ridgerun.com/wiki/index.php/GstShark_-_Tracers)

- todo: check works in non-gst-launch environment

<br>

<br>

# Debugging

## GST_DEBUG

[Documentation](https://gstreamer.freedesktop.org/documentation/tutorials/basic/debugging-tools.html?gi-language=c#the-debug-log)

Used to set the gstreamer specific log levels. Can log everything from basic errors/info logs to every event and message on the event bus.

These can be easily viewed using the [debug-viewer](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-devtools/debug-viewer). A small app for filtering and sorting through GST logs.

<br>

## DebugProbe plugin

TODO

## gst-devtools

[gst-devtools](https://gitlab.freedesktop.org/gstreamer/gstreamer/-/tree/main/subprojects/gst-devtools)

## GstShark

[todo - debugging](https://developer.ridgerun.com/wiki/index.php/GStreamer_Debugging#GStreamer_Element_Debugging)
[todo - gstd](https://github.com/RidgeRun/gstd-1.x)
[todo - gstd2](https://developer.ridgerun.com/wiki/index.php/GStreamer_Daemon)

> "GStreamer Daemon is gst-launch on steroids"

<br>

# Profiling and Optimisation

[GstShark](https://developer.ridgerun.com/wiki/index.php/Embedded_GStreamer_Performance_Tuning)

<br>

# Testing

[GstHarness](https://gstreamer.freedesktop.org/documentation/check/gstharness.html?gi-language=c)

[gstvalidate tutorial](https://blogs.igalia.com/aboya/2019/05/14/validateflow-a-new-tool-to-test-gstreamer-pipelines/)

<br>

# Environment Setup

A lot of your time will be spent reading, compiling and testing `c/c++`. I would suggest setting up your IDE to enable quick definition/ reference jumping (the promise of rust bindings doesn't let you escape cpp completely unfortunately).

todo: vscode devcontainer setup for gstreamer/nvidia
todo: nvim container setup

<br>

## Nvidia Deepstream

[Deepstream SDK](https://developer.nvidia.com/deepstream-sdk)