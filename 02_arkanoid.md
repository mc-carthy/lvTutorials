# Arkanoid

## Introduction

![Arcade version of Arkanoid](https://upload.wikimedia.org/wikipedia/en/a/a2/Arkanoid.png)

Arkanoid (or Breakout as it is often known) is a natural progression in terms of development from Pong. It shares several features that we have already tackled, such as the paddle movement, the ball bouncing off walls and colliding with paddles, as well as reacting to the ball falling out of the screen area. However it requires enough new features to represent a step up in development complexity. In this tutorial, we'll look into representing objects as Lua tables (a step towards an object-oriented approach), splitting our code across multiple files to aid mantainability and clarity, more advanced collision detection & resolution logic, and much more!

## Setup

As before, begin by creating a new folder, and in it create a `main.lua` file. Add necessary built-in LÖVE functions that we'll be using:

```lua
function love.load()

end

function love.update(dt)

end

function love.draw()

end

function love.keypressed(key)
    if key == "escape" then
        love.event.quit()
    end
end
```

Verify this works my navigating to the directory in your terminal/IDE and running `love .`. You should see a black untitled screen.

## Creating the paddle

