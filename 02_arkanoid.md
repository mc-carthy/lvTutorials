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

As discussed in the introduction, we are going to move towards representing objects in our game as Lua tables. If you're familiar with other programming languages, Lua tables behave in very much the same way as a Dictionary in C# or Python, a Map in Java/C++/C, a Hash in Ruby or an Object in JavaScript.

First of all, we will create an empty table representing the paddle (at the top of the file, outside of `love.load`):

```lua
paddle = {}
```

Then assign the required properties. In `love.load` add the following:

```lua
paddle.w = 100
paddle.h = 10
paddle.y = love.graphics.getHeight() - 100
paddle.x = love.graphics.getWidth() / 2 - paddle.w / 2
paddle.speed = 400
```

Next, we will add the logic to update and draw our paddle, in `love.update` and `love.draw` respectively. This is very similar to the equivilent functions in our previous tutorial, so I won't go into detail. We will also make use of the `math.clamp` function we wrote last time.

```lua
function love.update(dt)
    local dx = 0
    if love.keyboard.isDown("a") then
        dx = dx - 1
    end
    if love.keyboard.isDown("d") then
        dx = dx + 1
    end
    paddle.x = paddle.x + dx * paddle.speed * dt
    paddle.x = math.clamp(paddle.x, 0, love.graphics.getWidth() - paddle.w)
end

function love.draw()
    love.graphics.rectangle("fill", paddle.x, paddle.y, paddle.w, paddle.h)
end

function math.clamp(value, min, max)
    return math.max(math.min(value, max), min)
end
```

While the above code works perfectly fine, our `love.update` and `love.draw` functions will soon become much larger as we add more and more objects to our game.

Tables, as well as holding variables as values, can also hold functions. So, let's tidy up our code by extracting the logic for updating and drawing the paddle into functions, and assigning those functions to key's in the paddle table. We can then call those functions in `love.update` and `love.draw`.

```lua
function paddle.update(dt)
    local dx = 0
    if love.keyboard.isDown("a") then
        dx = dx - 1
    end
    if love.keyboard.isDown("d") then
        dx = dx + 1
    end
    paddle.x = paddle.x + dx * paddle.speed * dt
    paddle.x = math.clamp(paddle.x, 0, love.graphics.getWidth() - paddle.w)
end

function paddle.draw()
    love.graphics.rectangle("fill", paddle.x, paddle.y, paddle.w, paddle.h)
end

function love.update(dt)
    paddle.update(dt)
end

function love.draw()
    paddle.draw()
end
```

If you run the game now, you should be able to move the paddle left and right, without it going off screen.

Note: You may have noticed we've used a different notation for assigning functions to our table compared to assigning variables. These two code blocks are equivilant:

```lua
function paddle.draw()
    love.graphics.rectangle("fill", paddle.x, paddle.y, paddle.w, paddle.h)
end
```

```lua
paddle.draw = function ()
    love.graphics.rectangle("fill", paddle.x, paddle.y, paddle.w, paddle.h)
end
```

I'll be using the former as it is the same style used for the various functions provided by LÖVE (and yes, `love` is a table, and `load`, `update` and `draw` are all keys in that table).

Now with our core update and draw functions simply calling the relevant functions on the various objects in our game. Our code will be much neater and easier to follow. It will also make exctacting objects into their own files a much more simple process.