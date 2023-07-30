---
title: "Introduce the Chie app"
date: 2023-07-28T10:41:47+09:00
---

<style>
  h2 { color: #00B386!important }
  @media (prefers-color-scheme: light) {
    img.screenshot-dark { display: none }
  }
  @media (prefers-color-scheme: dark) {
    img.screenshot-light { display: none }
  }
</style>

Chie is a cross-platform desktop app for LLMs like ChatGPT, it has following
advantages over other similar apps:

* Open source and hackable.
* Support extensions.
* Use native UI (GTK on Linux, Cocoa on macOS, Win32 on Windows).

Visit [GitHub](https://github.com/chieapp/chie) for source code, and
[download links](https://github.com/chieapp/chie/releases/latest) for compiled
binary files.

<p class="opacity-70"><i>You can skip to <a href="#technical-details">the
technical details</a> if you are not really interested in another ChatGPT
app and just want to see what this app is written with.</i></p>

<img src="/homepage/screenshot.png" alt="Screenshot of the Chie app" class="screenshot-light">
<img src="/homepage/screenshot_dark.png" alt="Screenshot of the Chie app" class="screenshot-dark">

## Features

Chie supports a wide range of features commonly found in ChatGPT wrappers,
including streaming output, markdown rendering and code highlighting, multi-line
input, interruption and regeneration, multiple conversations and history,
automatic title generation, dark mode, system prompt, context length...

Apart from the "standard" features, below are some unique highlights from Chie:

### Multiple backends and multiple assistants

While Chie is perfectly suitable to be used as a simple ChatGPT wrapper, it is
also powerful to support pro users who use every AI service everywhere all at
once.

Currently Chie officially supports 3 backends of AI services:

* OpenAI API Key
* ChatGPT web backend (requires login with OpenAI account)
* Bing Chat (requires login with Microsoft account)

And it is possible to use other kinds of AI services by using
[extensions](#extensions), for example Google Bard with
[chie-extension-bard][bard].

It is also supported to add multiple backends of the same kind with different
credentials, and each backend can be used to create multiple AI assistants with
different kinds of parameters. For example, you can have one AI assistant help
editing emails with a specialized system prompt and high temperature, while
another AI assistant answering programming questions with GPT-4 model.

### Multiple windows

Chie is a multi-window app, apart from the default window which groups all
assistants in the same window, it is also possible to open each assistant in a
separate window.

![Multiple Windows](multi-windows.png)

### Multiple kinds of views

Chie provides 2 kinds of built-in view for interacting with AI assistants: a
`MultiChatsView` that supports multiple conversations, and a simple `ChatView`
that only shows one conversation. The latter is quite useful when users only
need one small window to do some quick lookups.

With extensions it is also possible to add other kinds of views. For example,
the [chie-extension-translator](https://github.com/chieapp/chie-extension-translator)
extension provides a `TranslatorView`, which is more handy than a chat interface
when users are using AI assistants to do translation work.

<img src="translator-view.png" alt="Translator View" style="max-width: 539px; margin: auto">

### Tray icons and keyboard shortcuts

To make access to assistants easier, Chie supports setting separate tray icons
and keyboard shortcuts for assistants.

<img src="assisant-settings.png" alt="Assistant Settings" style="max-width: 557px; margin: auto">

### Run ChatGPT plugins locally

Chie does not support OpenAI's ChatGPT plugins directly, but instead it is quite
easy to write Chie extensions that provide same functionalities.

For example [chie-extension-browser-tools](https://github.com/chieapp/chie-extension-browser-tools)
implements 2 tools for browsing and searching Internet, which can be used by the
AI assistants to provide up-to-date information.

<img src="tools.png" alt="Local Tools Execution" style="max-width: 450px; margin: auto">

The tools in Chie extensions run locally on user's machine, making it possible
for AI assistants to interact natively with the local machine, which is
important for certain kinds of tasks. For example, you don't have to upload
files to OpenAI's servers to run commands on them in the cloud, instead you can
make assistants handle the files directly on the local machine.

## Extensions

Chie supports external extensions written in JavaScript, it is still in rather
early stage so there is no extension store and the extensions can only be loaded
from local disk for now.

I have written a few sample extensions to show what they are capable of.

* [chie-extension-browser-tools](https://github.com/chieapp/chie-extension-browser-tools) -
  Add tools which can be used by assistants to browse and search Internet.
* [chie-extension-translator](https://github.com/chieapp/chie-extension-translator) -
  Add a new `TranslatorService` that implements translation assistants, along
  with a new `TranslatorView`.
* [chie-extension-bard](https://github.com/chieapp/chie-extension-bard) -
  Add Google Bard backend.

## Technical details

Chie is written in [TypeScript](https://www.typescriptlang.org) and runs on [a
slightly modified Node.js runtime](https://github.com/yue/yode), it uses native
UI  with help of the [Yue](https://github.com/yue/yue) library.

While using a native UI toolkit, a large part of Chie's UI is custom drawn,
because the standard UI elements are not rich enough to describe a modern chat
interface.

The chat messages are rendered in a system webview. To minimize performance
penalty the webview only renders a static HTML page with very limited
JavaScript code.

### Behind the decisions

When designing the Chie app, I wanted to achieve following targets:

* The project should be written in a language that is fast to develop with,
  while still being maintainable in the long run.
* Works on all major desktop environments.
* Easy to write extensions.
* Multi-window interface.
* Starts fast, responses fast, and uses reasonable resources.
* Capable of rendering a complex chat interface.

Browser-based solutions like Electron and Tauri would be a tough choice, with a
great team like VS Code it is surely possible to write a responsive Electron
app, but if I was going to simply write a web page with React and wrap it in a
webview, the performance and resources usage will likely be disappointing,
especially for a multi-window app.

In the meanwhile JavaScript with Node.js remains a very attractive platform,
which has one of the most active open source ecosystems and will greatly reduce
the development time.

So my choice is to write the entire app in Node.js, but creates the GUI with a
native UI library, which has a good balance between performance and development
speed.

### To webview, or not to webview

There is one problem remaining, how should the app render chat messages?

ChatGPT and most AI services use markdown for output, and rendering markdown is
basically rendering simple HTML documents. It is possible to write a simple
renderer without using a webview, but some markdown features will be very hard
to support. Remote images, code highlighting, math expressions -- the basic
features already requires a large investment of development time.

So to render the chat messages the most practical way is to use a webview, and
the performance is actually quite good.

Modern browsers (including IE11) are exceptionally good at rendering static HTML
pages. In Chie the chat messages are processed into a HTML string, and then
feeded into system webview. The time spent on initializing the webview and
rendering is almost nothing compared to the time spent on parsing markdown and
highlighting code snippets.

(There are also a few lightweight open source html renderer, but it is hard to
find one works out-of-box on all platforms, and I don't find a compelling reason
to use them.)

In the meanwhile, Chie has been designed to be able to use any kind of view for
displaying chat messages, for example the `TranslatorView` implemented in
[chie-extension-translator](https://github.com/chieapp/chie-extension-translator)
uses no webview at all. If some day an awesome markdown renderer implemented
with native UI emerged, we can always replace current implementation with it.

## Future plan

When started writing this app I was expecting a 3000~4000 lines project, but
soon it exceeded 9000 lines and there were still plenty of items on my TODO
list, and it started haunting me that no one might use this app at all. As a
result I freezed new features and began preparing for open sourcing, which also
took an unexpectedly long time.

So open sourcing this project is just the start of a very long journey -- and
considering the length of it, I am not quite willing to work voluntarily on it
forever.

Thus a business plan might be necessary, and there are a few obvious business
models:

* Provide a subscription plan for accessing premium models like GPT-4.
* Sell the app on app stores.
* Accept sponsorship from paid AI services in exchange of built-in integration
  inside Chie.

Apparently getting meaningful income from any of the plans requires a large
enough user base, while to get users I have to work very hard to improve the
app, which looks like a deadlock :). I guess I'll forget about money and just
enjoy open source for now.
