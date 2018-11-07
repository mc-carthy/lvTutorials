# Pong

*Before we get started, this particular tutorial will be aimed towards beginners (both in terms of general programming knowledge as well as the LÖVE framework & Lua), if you're not a beginner, feel free to skim through!*

Now, lets get started!

First of all, create a folder for our Pong game (Note: I will include a link to the repository so you can follow along and allow you to debug any errors you may come across). In that folder, create a file called `main.lua`. This is the entry point to our game (and in this tutorial, the entire game) that LÖVE will look for when it runs.

To test everything is running so far, navigate to your directory (either in IDE/text editor, or a terminal/command prompt) and run LÖVE. If you're using VSCode with the LÖVE plugin, you should be able to use Alt+L, alternatively you can add LÖVE to your path (see the 'Running Games' section [here](https://love2d.org/wiki/Getting_Started)), and type `love .` in your terminal.

You should see a plain black screen. Success!

## LÖVE built-in functions

There are several functions that LÖVE calls automatically, with no input from us. We'll see more as we progress through the tutorials, but the 3 main ones are:

`love.load` - This is called once when the game is run

`love.update` - This gets called as often as possible (up to 1000 times per second) and is where the majority of our logic will live.

`love.draw` - This gets called right after `love.update`, and is responsible for drawing shapes and images to the screen.

Since we'll use these 3 functions for every game we'll build, go ahead and add those functions above you your `main.lua` file as shown below. (I'll explain the `dt` in a later section):

```lua
function love.load()

end

function love.update(dt)

end

function love.draw()

end
```

## Drawing the ball

Now that we've got the skeleton of our game set up, let's actually add something that in some way resembles a game. Let's start by drawing the ball at the centre of the screen.

In your `love.draw` function, add the following code:

```lua
love.graphics.circle('fill', love.graphics.getWidth() / 2, love.graphics.getHeight() / 2, 5)
```

If you're using the LÖVE extension in VSCode, or you looking at the API reference [here](https://love2d.org/wiki/love.graphics.circle), you'll see that `love.graphics.circle` takes 4 paramters. A draw mode (whether we want an outline 'line', or a solid circle 'fill'), x and y positions of the centre, as well as the circle's radius.

Naturally, the x and y positions will vary once the game starts, so lets extract those values to variables, as well as the ball radius.

In your `love.load` function, add the following:

```lua
ballX = love.graphics.getWidth() / 2
ballY = love.graphics.getHeight() / 2
ballRad = 5
```
And update `love.draw` as follows:
```lua
love.graphics.circle('fill', ballX, ballY, ballRad)
```

If you run the game again, nothing will have changed, but we're now ready to move our ball. Let's give our ball a speed, in `love.load`, add the following variable:
```lua
ballSpeed = 100
```
And in `love.update`, add the following:
```lua
function love.update(dt)
  ballX = ballX + ballSpeed * dt
end
```

A few things to point out here, first of all, our ball is moving, which is great!
Secondly, if you're experienced in other programming languages, you might be thinking why we're not writing:
```lua
ballX += ballSpeed * dt
```
Unary operators such as +=, -=, ++, --, *=, /= etc. are not supported in Lua.

Finally, what is that `dt` doing? And how does it get there?

`dt` is short for delta time, that is the time between the previous call to `love.update` and the current one. As mentioned in the beginning, `love.update` and `love.draw` get called as often as possible. If we simply added the ball's speed to it's current position every time we called update, the speed of the ball would differ depending on how powerful your computer is. By multipling the speed by the time between frames, we can ensure that the speed is consistent whether you're game is running at 1000 frames per second, or 30.

## Keeping the ball within the screen

It's great that we've got our ball moving, until it moves off screen into oblivion that is. So let's confine our ball to the screen (at least until we get our paddles created).

In order to do this, we'll confine our ball's x position to stay between 0 and the screen width. We can do this by adding the following code to our update function:

```lua
if ballX < 0 then
  ballX = 0
  ballSpeed = -ballSpeed
end
if ballX > love.graphics.getWidth() then
  ballX = love.graphics.getWidth()
  ballSpeed = -ballSpeed
end
```

As well as flipping the speed value, we also need to place our ball back onto the boundary of our screen. If we did not do this, it's possible that the ball is so far beyond the edge of the screen, that when we flip the speed, it doesn't manage to get back into view. It will then continue to alternate it's direction for eternity, beyond the viewport.

In order to see this in action, comment out the lines setting `ballX`, and add the following line to our `love.draw` function

```lua
love.graphics.print(string.format("%.0f", ballX), 10, 10)
```

This will print the ball's x position as an integer to top-left corner of your screen. Try increasing the value of ballSpeed and watch as the ball starts to move so fast that it gets trapped outside of the screen.

## Another dimension, another dimension

Right now we have our ball moving from left to right, and back again. Not exactly going to make the most exciting competitive experience! So let's add a y component to our ball's movement. In the `load.load` function, replace the ballSpeed variable with the following:
```lua
ballSpeedX, ballSpeedY = love.math.random(100, 200), love.math.random(100, 200)
```
As you can see, in Lua we can assign multible variables at the same time. Here were setting both the x and y components to be a random value between 100 & 200 (as we're multiplying by dt, these values correspond to the speed in terms of number of pixels per second).

Now, let's actually use these new values. In our `love.update` function, replace all references to `ballSpeed` to `ballSpeedX`, and add the following line:
```lua
ballY = ballY + ballSpeedY * dt
```

You'll see the ball will now move in a random direction starting from the centre of the screen.

Let's keep the ball in bounds with it's y position. Similar to what we've done with the x position, add the following to the end of our update function:
```lua
if ballY < 0 then
    ballY = 0
    ballSpeedY = -ballSpeedY
end
if ballY > love.graphics.getHeight() then
    ballY = love.graphics.getHeight()
    ballSpeedY = -ballSpeedY
end
```

You'll see the ball now bounces off all 4 sides of the screen. Progress!

## Paddles