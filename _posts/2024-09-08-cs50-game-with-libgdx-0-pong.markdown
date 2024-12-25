---
title: 'CS50 Game in Java with LibGdx: 0 - Pong'
date: 2024-09-08 21:21:00 Z
categories:
- game
layout: post
lang: en
ref: cs50g0
---

*An adaptation of Harvard CS50 Game in Java with libGDX. The original course uses Lua and Love2D. 
All credits to Colton Ogden and David J. Malan.* 

Watch the course video [here](https://cs50.harvard.edu/games/2018/weeks/0/).  
Original notes in Lua with Love2D: [Lecture 0: Pong](https://cs50.harvard.edu/games/2018/notes/0/).  
Below are the notes adapted for Java with libGDX.

## Today’s Topics

- Java  
  This is the programming language that we’ll be using predominantly throughout the course. Java is a general purpose, high-level, class-based, object-oriented programming language.

- LibGDX  
  The primary game framework we’ll be using throughout the course. It works hand in hand with Java, and you can find documentation for it at [libgdx.com/dev](https://libgdx.com/dev).

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
- I recommend to go through the [official libGDX tutorial](https://libgdx.com/wiki/start/a-simple-game) first. Some elements will be redundant but at your learning stage, some repetition will not hurt.  

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
- First off, we’ll want to draw shapes to the screen (e.g., paddles and ball) so that the user can see the game.
- Next, we’ll want to control the 2D position of the paddles based on input, and implement collision detection between the 
paddles and ball so that each player can deflect the ball back toward their opponent.
- We’ll also need to implement collision detection between the ball and screen boundaries to keep the 
ball within the vertical bounds of the screen and to detect scoring events (outside horizontal bounds)
- At that point, we’ll want to add sound effects for when the ball hits paddles and walls, and for when a point is scored.
- Lastly, we’ll display the score on the screen so that the players don’t have to remember it during the game.

## pong-0 (“The Day-0 Update”) + pong-1 (“The Low-Res Update”)
- pong-0 simply prints “Hello Pong!” exactly in the center of the screen. This is not incredibly exciting, but it does showcase how to use LÖVE2D’s most important functions moving forward.
- pong-1 exhibits the same behavior as pong-0, but with much blurrier text.

Contrary to `Love2D`, these two steps are performed together because using a viewport is 
highly recommended in LibGdx. With a viewport, the `pong-1` step can be applied directly. [Learn more here](https://libgdx.com/wiki/start/a-simple-game#rendering).  
This section will be the heaviest but should be nothing new if you followed the [official libGDX tutorial](https://libgdx.com/wiki/start/a-simple-game) first.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/fc7606b046aa543c2bb4e83c88eda290d399f247).

### Important code 
- The `ApplicationListener` interface
  - A base interface that provides methods to override the behavior during the [life-cycle](https://libgdx.com/wiki/app/the-life-cycle) of the game application. 
  - `create` 
    - This method is used for initializing our game state at the very beginning of program execution. Whatever code we put here will be executed once when the application is created.
  - `render`
    - This method is called at each frame of program execution; dt (i.e., `Gdx.graphics.getDeltaTime()`) will be the elapsed time in seconds since the last frame, and we can use this to scale any changes in our game for even behavior across frame rates.
      Game logic updates are usually performed in this method.
- A viewport:
  ```java
  private FitViewport viewport;
  
  // in create()
  viewport = new FitViewport(WORLD_WIDTH, WORLD_HEIGHT, new OrthographicCamera(WORLD_WIDTH, WORLD_HEIGHT));
  viewport.getCamera().position.set(WORLD_WIDTH / 2, WORLD_HEIGHT / 2, 0);
  viewport.getCamera().update(); 
  
  // just before drawing
  viewport.apply(); 
  ```
  - A viewport controls how we see the game. It’s like a window from our world into the game world. The viewport controls how big the game “window” is and how it’s placed on our screen. There are many kinds of viewports. 
  The `FitViewport` ensures that no matter the size of our window, the full game will always be visible. 
  The parameters determine how large our visible game world will be in game units.
  The viewport uses the width and height `WORLD_WIDTH = 432;` and `WORLD_HEIGHT = 243;` this correspond to the game units.
  In the `Lwjgl3Launcher` file, we set the application to `configuration.setWindowedMode(1280, 720);`. This means we treat our game
  as if it were on a `432x243` window, while actually rendering it in a `1280x720` window. 
  Learn more in the [viewports and cameras wiki](https://libgdx.com/wiki/graphics/viewports). 
  - Always remember to update the viewport in the resize method:
    ```java
    @Override
      public void resize(int width, int height) {
          viewport.update(width, height, true);
      }
    ```
- A `BitmapFont`
  ```java
  private BitmapFont font;
  
  // in create()
  font = new BitmapFont();
  font.setColor(1,1,1,1);
  font.getRegion().getTexture().setFilter(TextureFilter.Nearest, TextureFilter.Nearest);
  ```
  A `BitmapFont` is used to render font (text) images. Drawing is then performed with: 
  ```
  font.draw(batch, "Hello pong", WORLD_WIDTH / 2 -40, WORLD_HEIGHT/2);
  ```
- A `Batch`
  ```java
  private SpriteBatch batch;
  
  // in create()
  batch = new SpriteBatch();
  ```
  A `Batch` is a common trick to reduce the load on the graphical processing unit (GPU), improving the FPS. 
  See [libgdx tutorial](https://libgdx.com/wiki/start/a-simple-game#rendering:~:text=Ever%20wonder%20why%20your%20favorite%20games%20sometimes%20have%20poor%20FPS%20or%20Frames%20Per%20Second%3F) to learn more. 
  The `SpriteBatch` combines draw calls together before sending them to the GPU. 
  `spriteBatch.setProjectionMatrix(viewport.getCamera().combined);`is first called to apply the `viewport` to the `SpriteBatch`. 
  This is necessary for the images to be shown in the correct place.  
  Then the `draw` calls are performed between the `begin` and `end` method calls of the batch.
  ```java
  batch.begin();
  // drawing here
  batch.end();
  ```
- `Gdx.input.isKeyPressed(Keys.<SOME_KEY>)`   
  Returns true if the key is pressed. It allows us to receive inputs from the keyboard for our game.
- `Gdx.app.exit()`   
  Terminates the application upon execution.
- We add a way to quit the game via user input, using the two functions above:
  ```java
  if (Gdx.input.isKeyPressed(Keys.ESCAPE)) {
    Gdx.app.exit();
  }
  ```

## pong-2 (“The Rectangle Update”)
- pong-2 produces a more complete, albeit static image of what our Pong program should look like.
  
[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/7e6d9d046cea478dd482435b7c188acb8a7ee268).

## Important code
- A new extension [gdx-freetype](https://libgdx.com/wiki/extensions/gdx-freetype) to draw text.   
  In the previous section, we used a `BitmapFont` to render text. We now want to use a front from 
  a `.ttf` file. 
  - We add the [gdx-freetype](https://libgdx.com/wiki/extensions/gdx-freetype) extension to the project in `core/build.gradle`.
    ```gradle
    api "com.badlogicgames.gdx:gdx-freetype:$gdxVersion"
    ```
  - We add `font.ttf` to the `assets` folder.
  - We can then generate a `BitmapFont` of a given size on the fly from `ttf` files.
    ```java
    private BitmapFont smallFont;
    
    // in create()
    final FreeTypeFontGenerator generator = new FreeTypeFontGenerator(Gdx.files.internal("font.ttf"));
    final FreeTypeFontParameter parameter = new FreeTypeFontParameter();
    parameter.size = 8;
    smallFont = generator.generateFont(parameter);
    smallFont.getRegion().getTexture().setFilter(TextureFilter.Nearest, TextureFilter.Nearest);
    smallFont.setColor(1, 1,1,1);
    generator.dispose(); // don't forget to dispose to avoid memory leaks!
    ```
  - To center the text, we compute the `GlyphLayout`, which corresponds the rendered text layout, then we use its width.
    ```java
    final GlyphLayout layout = new GlyphLayout(smallFont, "Hello Pong!");
    smallFont.draw(batch, layout, (WORLD_WIDTH - layout.width) / 2, WORLD_HEIGHT - 20);
    ```
    As you can see, are shifting “Hello Pong!” higher up on the screen.
- A `ShapeRenderer` to draw rectangles
  ```java
  private ShapeRenderer shape;
  
  // in create()
  shape = new ShapeRenderer();
  
  // in render() --> draw()
  shape.setProjectionMatrix(viewport.getCamera().combined);
  shape.begin(ShapeType.Filled);
  shape.setColor(Color.WHITE);
  shape.rect(10, WORLD_HEIGHT - 30 - 20, 5, 20);
  shape.rect(WORLD_WIDTH - 10 - 5, 30, 5, 20);
  shape.rect(WORLD_WIDTH / 2 -2 , WORLD_HEIGHT / 2 - 2, 4, 4);
  shape.end();
  ```
  Similarly to `SpriteBatch` for textures, a shape has a `begin` and `end` methods to batch drawings.  
  The paddles are positioned on opposing ends of the screen, and the ball in the center.

## pong-3 (“The Paddle Update”)
- pong-3 adds interactivity to the Paddles by letting us move them up and down using the w and s keys 
  for the left Paddle and the up and down keys for the right Paddle.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/45eaeb9fb37e96aad37f0e3dd6de3e33103c477d).

### Important code

- You’ll notice we’ve added a new constant near the top of main.lua:
  `PADDLE_SPEED = 200`
  This is an arbitrary value that we’ve chosen for our paddle speed. It will be scaled by DeltaTime, so it’ll be multiplied by how much time has passed (in terms of seconds) since the last frame, so that our paddle movement will remain consistent regardless of how quickly or slowly our computer is running.
- You’ll also find some new variables in `create()`
  ```java
  // in create()
  parameter.size = 32;
  scoreFont = generator.generateFont(parameter);
  scoreFont.getRegion().getTexture().setFilter(TextureFilter.Nearest, TextureFilter.Nearest);
  scoreFont.setColor(Color.WHITE);
  
  player1Score = 0;
  player2Score = 0;
  player1Y = WORLD_HEIGHT - 30 - 20;
  player2Y = 30;
  ```
  In particular, we’ve created a new font object that is of larger size so that we can display each 
  player’s score more visibly on the screen, and allocated two variables for the purpose of 
  scorekeeping. The last two variables will keep track of each paddle’s vertical position, since 
  the paddles will be able to move up and down.
- Next, you’ll see that we’ve finally defined behavior for `render() -> input()`:
  ```java
  if (Gdx.input.isKeyPressed(Keys.DOWN)) {
    player2Y -= PADDLE_SPEED * Gdx.graphics.getDeltaTime();
  } else if (Gdx.input.isKeyPressed(Keys.UP)) {
    player2Y += PADDLE_SPEED * Gdx.graphics.getDeltaTime();
  }
  if (Gdx.input.isKeyPressed(Keys.S)) {
    player1Y -= PADDLE_SPEED * Gdx.graphics.getDeltaTime();
  } else if (Gdx.input.isKeyPressed(Keys.Z) || Gdx.input.isKeyPressed(Keys.W)) {
    // Z or W to be compatible with both AZERTY and QWERTY in a simple way
    player1Y += PADDLE_SPEED * Gdx.graphics.getDeltaTime();
  }
  ```
  Here, we’ve implemented a way for each player to move their paddle.
- Lastly, in `render() --> draw()` you’ll see that we’ve added code for displaying the score on 
  the screen:
  ```java
  scoreFont.draw(batch, String.valueOf(player1Score), WORLD_WIDTH / 2 - 50, WORLD_HEIGHT - (WORLD_HEIGHT / 3));
  scoreFont.draw(batch, String.valueOf(player2Score), WORLD_WIDTH / 2 + 30, WORLD_HEIGHT - (WORLD_HEIGHT / 3));
  ```
  
## pong-4 (“The Ball Update”)
Coming soon


  
   
  


