<p align="center">
  <img width="200px" src="/Designs/YBird.gif" alt="YBird" />
</p>

# [Y](https://yapp.io)

[![Open in Hanzo](https://hanzo.app/badges/open-in-hanzo.svg)](https://hanzo.app/dev/open?repo=github.com/zeekay/yapp&ref=main&action=open) [![Edit with Hanzo](https://hanzo.app/badges/edit-with-hanzo.svg)](https://hanzo.app/dev/open?repo=github.com/zeekay/yapp&ref=main&action=edit)

[![Downloads](https://img.shields.io/github/downloads/zeekay/yapp/total.svg)](https://github.com/zeekay/yapp/releases/latest)
[![Build Status](https://img.shields.io/bitrise/716921b669780314/master?token=3pMiCb5dpFzlO-7jTYtO3Q)](https://app.bitrise.io/app/716921b669780314)
[![Donate](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://donate.zoo.ngo)

Y is a lightweight AI manager for macOS. It keeps the history of what you AI
and lets you quickly navigate, search, and use previous AI contents.

Thanks to integration with Claude and Langchain, users can easily access
best-in-class API straight from the clipboard.

Y works on macOS Mojave 10.14 or higher.

<!-- vim-markdown-toc GFM -->

* [Features](#features)
* [Install](#install)
* [Usage](#usage)
* [Advanced](#advanced)
    * [Ignore Copied Items](#ignore-copied-items)
    * [Ignore Custom Copy Types](#ignore-custom-copy-types)
* [FAQ](#faq)
    * [Why doesn't it paste when I select an item in history?](#why-doesnt-it-paste-when-i-select-an-item-in-history)
* [Motivation](#motivation)
* [License](#license)

<!-- vim-markdown-toc -->

## Features
* Lightweight and fast
* Keyboard-first
* Secure and private
* Native UI for Claude
* SERP integrated for realtime internet queries
* Langchain for linking up models
* Chroma for vector database
* Open source and free


## How it works

Here is an explanation of how the API for YApp works:

The YApp API is designed to allow the YApp native clipboard manager to seamlessly integrate with the Claude AI assistant. When a user copies text to their clipboard in YApp, it sends that text to the /ask endpoint of the API.

The /ask endpoint takes the input text, creates a conversation history by looking at previous messages with the 'user' role, and uses that to construct a prompt for Claude. It uses the LangChain library to simplify the process of chatting with Claude. The prompt contains the chat history and the user's input text.

Claude then generates a response, which is returned by the API as a JSON object. This allows YApp to display Claude's response in the app interface, making it feel like a real-time conversation.

The /search endpoint works similarly, but instead of just chatting with Claude, it uses a tool to run a web search using SerpAPI. This allows Claude to provide more informed responses by looking up additional context on the web when needed.

The API uses Flask to handle the web requests and responses. It connects Claude and LangChain behind the scenes to add conversational AI abilities to YApp. The real-time nature of the API allows YApp to feel very responsive rather than needing to wait for network requests on each user interaction.

Overall, the API enables seamless integration between YApp and Claude using modern AI techniques. It expands the capabilities of YApp's clipboard manager by allowing users to have an intelligent conversation about the content they copy and paste.

## Install

Download the latest version from the
[releases](https://github.com/zeekay/yapp/releases/latest) page.


```sh
git clone https://github.com/zeekay/yapp
```

## Demo

For hackathon demo purposes, manually run `api.py` locally before using Mac app.
```
python api.py
```

Local access to server is available thru /ask and /search endpoints:
```
curl -X POST -H "Content-Type: application/json" -d '{"message":[{"role":"user","content":"what day is it?"}]}' http://localhost:5000/ask
```

## Usage

1. <kbd>SHIFT (⇧)</kbd> + <kbd>COMMAND (⌘)</kbd> + <kbd>C</kbd> to popup Y or click on its icon in the menu bar.
2. Type what you want to find.
3. To select the history item you wish to copy, press <kbd>ENTER</kbd>, or click the item, or use <kbd>COMMAND (⌘)</kbd> + `n` shortcut.
4. To choose the history item and paste, press <kbd>OPTION (⌥)</kbd> + <kbd>ENTER</kbd>, or <kbd>OPTION (⌥)</kbd> + <kbd>CLICK</kbd> the item, or use <kbd>OPTION (⌥)</kbd> + `n` shortcut.
5. To choose the history item and paste without formatting, press <kbd>OPTION (⌥)</kbd> + <kbd>SHIFT (⇧)</kbd> + <kbd>ENTER</kbd>, or <kbd>OPTION (⌥)</kbd> + <kbd>SHIFT (⇧)</kbd> + <kbd>CLICK</kbd> the item, or use <kbd>OPTION (⌥)</kbd> + <kbd>SHIFT (⇧)</kbd> + `n` shortcut.
6. To delete the history item, press <kbd>OPTION (⌥)</kbd> + <kbd>DELETE (⌫)</kbd>.
7. To see the full text of the history item, wait a couple of seconds for tooltip.
8. To pin the history item so that it remains on top of the list, press <kbd>OPTION (⌥)</kbd> + <kbd>P</kbd>. The item will be moved to the top with a random but permanent keyboard shortcut. To unpin it, press <kbd>OPTION (⌥)</kbd> + <kbd>P</kbd> again.
9. To clear all unpinned items, select _Clear_ in the menu, or press <kbd>OPTION (⌥)</kbd> + <kbd>COMMAND (⌘)</kbd> + <kbd>DELETE (⌫)</kbd>. To clear all items including pinned, select _Clear_ in the menu with  <kbd>OPTION (⌥)</kbd> pressed, or press <kbd>SHIFT (⇧)</kbd> + <kbd>OPTION (⌥)</kbd> + <kbd>COMMAND (⌘)</kbd> + <kbd>DELETE (⌫)</kbd>.
10. To disable Y and ignore new copies, click on the menu icon with <kbd>OPTION (⌥)</kbd> pressed.
11. To ignore only the next copy, click on the menu icon with <kbd>OPTION (⌥)</kbd> + <kbd>SHIFT (⇧)</kbd> pressed.
12. To customize the behavior, check "Preferences..." window, or press <kbd>COMMAND (⌘)</kbd> + <kbd>,</kbd>.

## Advanced

### Ignore Copied Items

You can tell Y to ignore all copied items:

```sh
defaults write org.yapp.Y ignoreEvents true # default is false
```

This is useful if you have some workflow for copying sensitive data. You can set `ignoreEvents` to true, copy the data and set `ignoreEvents` back to false.

You can also click the menu icon with <kbd>OPTION (⌥)</kbd> pressed. To ignore only the next copy, click with <kbd>OPTION (⌥)</kbd> + <kbd>SHIFT (⇧)</kbd> pressed.

### Ignore Custom Copy Types

By default Y will ignore certain copy types that are considered to be confidential
or temporary. The default list always include the following types:

* `org.nspasteboard.TransientType`
* `org.nspasteboard.ConcealedType`
* `org.nspasteboard.AutoGeneratedType`

Also, default configuration includes the following types but they can be removed
or overwritten:

* `com.agilebits.onepassword`
* `com.typeit4me.clipping`
* `de.petermaurer.TransientPasteboardType`
* `Pasteboard generator type`
* `net.antelle.keeweb`

You can add additional custom types using preferences or `defaults`:

```sh
defaults write org.yapp.Y ignoredPasteboardTypes -array-add "com.myapp.CustomType"
```

If you need to find what custom types are used by an application, you can use
free application [Pasteboard-Viewer](https://github.com/sindresorhus/Pasteboard-Viewer).
Simply download the application, open it, copy something from the application you
want to ignore and look for any custom types in the left sidebar.

If you accidentally removed default types, you can restore the original configuration:

```sh
defaults write org.yapp.Y ignoredPasteboardTypes -array "de.petermaurer.TransientPasteboardType" "com.typeit4me.clipping" "Pasteboard generator type" "com.agilebits.onepassword" "net.antelle.keeweb"
```
## FAQ

### Why doesn't it paste when I select an item in history?

1. Make sure you have "Paste automatically" enabled in Preferences.
2. Make sure "Y" is added to System Settings -> Privacy & Security -> Accessibility.

## Motivation

There are dozens of similar applications out there, so why build another?
Over the past years since I moved from Linux to macOS, I struggled to find
a clipboard manager that is as free and simple as [Parcellite](http://parcellite.sourceforge.net),
but I couldn't. So I've decided to build one.

Also, I wanted to learn Swift and get acquainted with macOS application development.

## License

[MIT](./LICENSE)
