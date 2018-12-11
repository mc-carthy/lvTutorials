# An Introduction to Game Development with LÖVE

## What is LÖVE?

LÖVE is a lightweight 2D game development framework. It allows us to have a large amount of control over how we can structure our code while abstracting away the very low-level tasks such as rendering, audio and keyboard/controller input. LÖVE uses SDL & OpenGL to achieve this which also allows for exporting of games made with LÖVE to all sorts of platforms. Including Windows/OSX/Linux, iOS & Android as well as ARM devices such as the RaspberryPi.

Before we get into building our games, here's a quick summary of the tools we're going to be using to build our games, and how to get up and running.

You can download LÖVE [here](https://love2d.org/). 

Lua is a relatively simple and lightweight programming language that we will be using to program our games. If you have previous experience with programming languages (especially other scripting languages), you should feel right at home. If you're new to programming, it's a great language to start with. There are relatively few built-in functions and data types, so hopefully you'll get up to speed quite quickly! If you want to read more on Lua, the first edition of Programming in Lua is available for free online [here](https://www.lua.org/pil/contents.html). But we'll cover the essentials in the following tutorials.

In order to write our games, we'll need a text editor. You can use whatever you like (notepad, SublimeText, vim etc.), but I recommend using [VSCode](https://code.visualstudio.com/) which has some very useful plugins including [Lua language support](https://marketplace.visualstudio.com/items?itemName=keyring.Lua) as well as a [LÖVE plugin](https://marketplace.visualstudio.com/items?itemName=pixelbyte-studios.pixelbyte-love2d) that includes snippets for the LÖVE API, as well as allowing your project to be run directly from VSCode (you'll need to configure the settings of that plugin to point to where you installed LÖVE).

With that all said and done, let's get coding! 

In our first tutorial, we'll create Pong, which can be found [here](https://mccarthy.tech/#/blogs/tutorials-pong)