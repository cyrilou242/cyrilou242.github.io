---
title: 'CS50 Game with LibGdx - 0: Pong'
date: 2024-09-08 21:21:00 Z
categories:
- game
tags:
- game
lang: en
ref: cs50g0
---

An adaptation of Harvard CS50 Game for LibGDX.\
[Lecture 0: Pong](https://cs50.harvard.edu/games/2018/notes/0/).\
Watch the video [here](https://cs50.harvard.edu/games/2018/weeks/0/).

# Today’s Topics

* Java
  * This is the programming language that we’ll be using predominantly throughout the course. Lua is a dynamic scripting language similar to Python and JavaScript.

* LÖVE2D

  * The primary game framework we’ll be using throughout the course. It works hand in hand with Lua, and you can find documentation for it at love2d.org/wiki/Main_Page.

* Drawing Shapes and Text

  * Two of the most basic principles of game development, being able to draw shapes and text is what will allow us to render our game on a screen.

* DeltaTime and Velocity

  * DeltaTime, arguably one of the most important variables that we keep track of in any game framework, is the time elapsed since the last frame of execution in our game. LÖVE2D measures DeltaTime in terms of seconds, so we’ll see how this concept relates to velocity.

* Game State

  * Every game is composed of a series of states (e.g., the title screen state, gameplay state, menu state, etc.), so it will be important to understand this concept since we’ll want different rendering logic and update logic for each state.

* Basic OOP (Object-Oriented Programming)

  * The use of Object-Oriented Programming will allow us to encapsulate our data and game objects such that each object in our game will be able to keep track of all the information that is relevant to it, as well as have access to specific functions that are unique to it.

* Box Collision (Hitboxes)

  * Understanding the concept of box collision will be necessary in order to bring Pong to life, since we’ll need to be able to “bounce” a ball back and forth between two paddles. The ball and paddles will be rectangular, so we’ll focus on “Axis-Aligned Bounding Boxes,” which will allow us to calculate collisions more simply.

* Sound Effects (with bfxr)

  * Lastly, we’ll learn how to polish up our game with sound effects in order to make it more enticing and immersive.