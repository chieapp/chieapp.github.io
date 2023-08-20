---
title: "Minimalist Approach to Using ChatGPT"
date: 2023-08-20T09:07:45+09:00
---

<style>
  img {
    width: 100%;
    height: auto;
    margin: auto;
  }
</style>

I use ChatGPT as a swiss army knife: it is a search engine, a grammer checker,
a dictionary, a translator, a pair programmer... So the efficiency of invoking
and interacting with ChatGPT directly affects my productivity.

Therefore I wrote the Chie app, designed for minimalists and hackers like
myself.

## Minimal interface

In Chie the window can be reduced to a bare chat window, with only conversation
history and an input box.

<img src="minimal-chat.png" alt="Minimal Chat Interface" style="max-width: 493px">

<details>
<summary>Steps of setup:</summary>

1. Create an API credential, but deselect the option to create assistant.

<img src="step-1.png" alt="Create API Credential Window" style="max-width: 541px">

2. Create an assistant (by clicking the plus button in dashboard window, or via
Menu: Assistants -> New Assistant), and choose "ChatService" for the "Service".

<img src="step-2.png" alt="Create Assistant Window" style="max-width: 552px">

3. After creating the assistant, you can popup the assistant in a new window.

<img src="step-3.png" alt="Create Assistant Window" style="max-width: 209px">

</details>

## Access from tray icon

There is also a tray icon where you can invoke any assistant.

<img src="tray-icon-menu.png" alt="Menu of Tray Icon" style="max-width: 179px">

The tray icon can be turned off in settings if you don't like it, and on macOS
there is also an option to turn off the dock icon too.

<img src="settings.png" alt="Settings Window" style="max-width: 612px">

To make things even more accessible, you can configure each assistant to show
its own tray icon.

<img src="assistant-tray.png" alt="Tray Icons for Assistants" style="max-width: 556px">

## Keyboard shortcuts

The assistant windows can also have their own global keyboard shortcuts.

<img src="keyboard-shortcut.png" alt="Keyboard Shortcut for Assistants" style="max-width: 662px">

And inside the window there are also a rich set of keyboard shortcuts for common
operations.

<img src="menu-bar-keys.png" alt="Menu Bar" style="max-width: 403px">
