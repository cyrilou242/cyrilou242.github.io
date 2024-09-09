---
title: 'CS50 Game in Java with LibGdx: 0 - Pong'
date: 2024-09-08 21:21:00 Z
categories:
- game
layout: post
lang: en
ref: cs50g0
---

An adaptation of Harvard CS50 Game for LibGDX. [Lecture 0: Pong](https://cs50.harvard.edu/games/2018/notes/0/). Watch the video [here](https://cs50.harvard.edu/games/2018/weeks/0/).

## Today’s Topics

- Java  
  This is the programming language that we’ll be using predominantly throughout the course. Java is a dynamic scripting language similar to Python and JavaScript.

- LibGDX  
  The primary game framework we’ll be using throughout the course. It works hand in hand with Java, and you can find documentation for it at libgdx.com/dev.

- Drawing Shapes and Text  
  Two of the most basic principles of game development, being able to draw shapes and text is what will allow us to render our game on a screen.

- DeltaTime and Velocity  
  DeltaTime, arguably one of the most important variables that we keep track of in any game framework, is the time elapsed since the last frame of execution in our game. LibGDX measures DeltaTime in terms of seconds, so we’ll see how this concept relates to velocity.

- Game State  
  Every game is composed of a series of states (e.g., the title screen state, gameplay state, menu state, etc.), so it will be important to understand this concept since we’ll want different rendering logic and update logic for each state.

- Basic OOP (Object-Oriented Programming)  
  The use of Object-Oriented Programming will allow us to encapsulate our data and game objects such that each object in our game will be able to keep track of all the information that is relevant to it, as well as have access to specific functions that are unique to it.

- Box Collision (Hitboxes)  
  Understanding the concept of box collision will be necessary in order to bring Pong to life, since we’ll need to be able to “bounce” a ball back and forth between two paddles. The ball and paddles will be rectangular, so we’ll focus on “Axis-Aligned Bounding Boxes,” which will allow us to calculate collisions more simply.

- Sound Effects (with bfxr)  
  Lastly, we’ll learn how to polish up our game with sound effects in order to make it more enticing and immersive.

## Installing libGDX
- Before you start following along with the rest of the lecture, be sure to have libGDX setup on your machine, 
  which you can do through the following [libgdx.com/wiki/start/setup](https://libgdx.com/wiki/start/setup).


## Downloading Demo Code
Next, be sure to download the code for today’s lecture, which you can find at: [github.com/cyrilou242/cs50-pong-java-libgdx](https://github.com/cyrilou242/cs50-pong-java-libgdx).  
This should make it easier to follow along without having to focus on matching every keystroke in real time.  
Each commit corresponds to a step pong-0, pong-1, pong-2, etc in [the video](https://cs50.harvard.edu/games/2018/weeks/0/). Make sure to know how to get to a specific commit in git. 

## What is Java?
If you read this Java/LibGdx article instead of the Lua/Love2D one, you must know what is Java. Just in case: https://en.wikipedia.org/wiki/Java_(programming_language).

## What is libGDX?
libGDX is a free cross-platform Java game development framework based on OpenGL (ES) that 
works on Windows, Linux, macOS, Android, your browser and iOS.
It contains modules for graphics, keyboard input, math, audio, windowing, physics, 
and much more.

## What is a game loop?
A game, fundamentally, is an infinite loop, like a while(true) or a while(1). During every iteration of that loop, we’re repeatedly performing the following set of steps:
- First, we’re processing input. That is to say, we’re constantly checking: has the user pressed a key on the keyboard, moved the joystick, moved/clicked the mouse, etc.?
- Second, we need to respond to that input from the previous step by updating anything in the game that depends on that input (i.e., tracking movement, detecting collisions, etc.).
- Third, we need to re-render anything that was updated in the previous step, so that the user can see visually on the screen that the game has changed and feel a sense of interactivity.

![the game loop](/assets/images/cs50-game-with-libgdx-0-pong/game_loop.png)
Photo taken from [gameprogrammingpatterns.com/game-loop.html](gameprogrammingpatterns.com/game-loop.html), where you can read more about game loops.

## 2D Coordinate System
- In the context of 2D games, the most fundamental way of looking at the world is by using the 2D coordinate system.
- Similar to the traditional coordinate system you might’ve used in math class, 
  the 2D coordinate system we’re referring to here is a system in which objects have an X and Y coordinate (X, Y) and are drawn accordingly, 
  with the origin (0,0) being the bottom-left of the system.
- **Caution!** The libGDX coordinate system is different from Lua Love2D's one which has the origin (0,0) in the top-left.   
  Love2D's follows the classic representation of a display - which is usually also closest to the device/OS specific implementation - while libGdx follows the OpenGL way. Learn more [in the libGDX doc](https://libgdx.com/wiki/articles/coordinate-systems#screen-or-image-coordinates).

![2 coordinate systems](/assets/images/cs50-game-with-libgdx-0-pong/coordinate_systems.png)
*Left: the libGDX coordinate system. Right: the Love2D's coordinate system.*

## Today’s Goal
We are aiming to recreate “Pong,” a simple 2 player game in which one player has a paddle on the left side of the screen, 
the other player has a paddle on the right side of the screen, and the first player to score 10 times on their opponent wins. 
A player scores by getting the ball past the opponent’s paddle and into their “goal” (i.e., the edge of the screen).

![pong example](/assets/images/cs50-game-with-libgdx-0-pong/pong_example.png){: width="700" }

# Lecture’s Scope
First off, we’ll want to draw shapes to the screen (e.g., paddles and ball) so that the user can see the game.
Next, we’ll want to control the 2D position of the paddles based on input, and implement collision detection between the 
paddles and ball so that each player can deflect the ball back toward their opponent.
We’ll also need to implement collision detection between the ball and screen boundaries to keep the 
ball within the vertical bounds of the screen and to detect scoring events (outside horizontal bounds)
At that point, we’ll want to add sound effects for when the ball hits paddles and walls, and for when a point is scored.
Lastly, we’ll display the score on the screen so that the players don’t have to remember it during the game.
