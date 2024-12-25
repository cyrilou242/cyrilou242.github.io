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
  Two of the most basic principles of game development, being able to draw shapes and text is what 
  will allow us to render our game on a screen.

- DeltaTime and Velocity  
  DeltaTime, arguably one of the most important variables that we keep track of in any game 
  framework, is the time elapsed since the last frame of execution in our game. LibGDX measures 
  DeltaTime in terms of seconds, so we’ll see how this concept relates to velocity.

- Game State  
  Every game is composed of a series of states (e.g., the title screen state, gameplay state, menu state, etc.), so it will be important to understand this concept since we’ll want different rendering logic and update logic for each state.

- Basic OOP (Object-Oriented Programming)  
  The use of Object-Oriented Programming will allow us to encapsulate our data and game objects 
  such that each object in our game will be able to keep track of all the information that is relevant to it, as well as have access to specific functions that are unique to it.

- Box Collision (Hitboxes)  
  Understanding the concept of box collision will be necessary in order to bring Pong to life, since we’ll need to be able to "bounce" a ball back and forth between two paddles. The ball and paddles will be rectangular, so we’ll focus on "Axis-Aligned Bounding Boxes," which will allow us to calculate collisions more simply.

- Sound Effects (with bfxr)  
  Lastly, we’ll learn how to polish up our game with sound effects in order to make it more 
  enticing and immersive.

## Installing libGDX
- Before you start following along with the rest of the lecture, be sure to have libGDX setup on your machine, 
  which you can do through the following [libgdx.com/wiki/start/setup](https://libgdx.com/wiki/start/setup).
- I recommend to go through the [official libGDX tutorial](https://libgdx.com/wiki/start/a-simple-game) first. Some elements will be redundant but at your learning stage, some repetition will not hurt.  

## Downloading Demo Code
Next, be sure to download the code for today’s lecture, which you can find at: [github.com/cyrilou242/cs50-pong-java-libgdx](https://github.com/cyrilou242/cs50-pong-java-libgdx).  
This should make it easier to follow along without having to focus on matching every keystroke in real time.  
Each commit corresponds to a step pong-0, pong-1, pong-2, etc in [the video](https://cs50.harvard.edu/games/2018/weeks/0/). Make sure to know how to get to a specific commit in git. 

## What is Java?
If you read this Java/LibGdx article instead of the Lua/Love2D one, you do know what is [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) right? Right?

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
We are aiming to recreate "Pong," a simple 2 player game in which one player has a paddle on the left side of the screen, 
the other player has a paddle on the right side of the screen, and the first player to score 10 times on their opponent wins. 
A player scores by getting the ball past the opponent’s paddle and into their "goal" (i.e., the edge of the screen).

![pong example](/assets/images/cs50-game-with-libgdx-0-pong/pong_example.png){: width="700" }

# Lecture’s Scope
- First off, we’ll want to draw shapes to the screen (e.g., paddles and ball) so that the user can see the game.
- Next, we’ll want to control the 2D position of the paddles based on input, and implement collision detection between the 
paddles and ball so that each player can deflect the ball back toward their opponent.
- We’ll also need to implement collision detection between the ball and screen boundaries to keep the 
ball within the vertical bounds of the screen and to detect scoring events (outside horizontal bounds)
- At that point, we’ll want to add sound effects for when the ball hits paddles and walls, and for when a point is scored.
- Lastly, we’ll display the score on the screen so that the players don’t have to remember it during the game.

## pong-0 ("The Day-0 Update") + pong-1 ("The Low-Res Update")
- pong-0 simply prints "Hello Pong!" exactly in the center of the screen. This is not incredibly exciting, but it does showcase how to use LÖVE2D’s most important functions moving forward.
- pong-1 exhibits the same behavior as pong-0, but with much blurrier text.

Contrary to `Love2D`, these two steps are performed together because using a viewport is 
highly recommended in LibGdx. With a viewport, the `pong-1` step can be applied directly. [Learn more here](https://libgdx.com/wiki/start/a-simple-game#rendering).  
This section will be the heaviest but should be nothing new if you followed the [official libGDX tutorial](https://libgdx.com/wiki/start/a-simple-game) first.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/fc7606b046aa543c2bb4e83c88eda290d399f247).

### Important code 
- The `ApplicationListener` interface
  - A base interface that provides methods to override the behavior during the [life-cycle](https://libgdx.com/wiki/app/the-life-cycle) of the game application. 
  - `create` 
    - This method is used for initializing the game state at the very beginning of program execution. 
      Whatever code we put here will be executed once when the application is created.
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
  - A viewport controls how we see the game. It’s like a window from our world into the game world. 
  The viewport controls how big the game "window" is and how it’s placed on our screen. There are many kinds of viewports. 
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

## pong-2 ("The Rectangle Update")
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
    As you can see, are shifting "Hello Pong!" higher up on the screen.
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

## pong-3 ("The Paddle Update")
- pong-3 adds interactivity to the Paddles by letting us move them up and down using the w and s keys 
  for the left Paddle and the up and down keys for the right Paddle.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/45eaeb9fb37e96aad37f0e3dd6de3e33103c477d).

### Important code

- You’ll notice we’ve added a new constant near the top of main.lua:
  `PADDLE_SPEED = 200`
  This is an arbitrary value that we’ve chosen for the paddle speed. It will be scaled by 
  DeltaTime, so it’ll be multiplied by how much time has passed (in terms of seconds) since the 
  last frame, so that the paddle movement will remain consistent regardless of how quickly or 
  slowly the computer is running.
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
  
## pong-4 ("The Ball Update")
- pong-4 adds motion to the Ball upon the user pressing enter.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/ba69e2e1a8074d6b64e314afb789ae3a5cea1cbe#diff-56f635608c63a855eaca50332e022c9a778c605578313c3881923e84ab8e12bbR157)

### Important code

- Random starting speed for the ball
  ```java
  randomGen = new Random();
  
  // in create()
  gameState = GameState.START;
  
  // in create() --> initBall()
  ballX = WORLD_WIDTH / 2 - 2;
  ballY = WORLD_HEIGHT / 2 - 2;
  ballDx = randomGen.nextBoolean() ? 100 : -100;
  ballDy = randomGen.nextInt(0, 101) - 50;
  ```
  `ballX` and `ballY` will keep track of the ball position, 
  while `ballDX` and `ballDY` will keep track of the ball velocity. 
  `gameState` will serve as a rudimentary "state machine", such that we’ll cycle it through the 
  different states of our game (start, play, etc.)
- In `render() -> input()`, we tweak the code for paddle movement by wrapping it around the 
  `Math.max()` and `Math.min()` functions to ensure that the paddles can’t move beyond the edges 
  of the screen.
- We also add new code to ensure the ball can only move when we are in the "play" state:
  ```java
  if (gameState == GameState.PLAY) {
    ballX += ballDx * Gdx.graphics.getDeltaTime();
    ballY += ballDy * Gdx.graphics.getDeltaTime();
  }
  ```
- Following this, we add 
  functionality to launch the game (thus transitioning from the "START" state to the "PLAY" state) 
  and implement ball movement mechanics:
  ```java
  if (Gdx.input.isKeyJustPressed(Keys.ESCAPE)) {
    Gdx.app.exit();
  } else if (Gdx.input.isKeyJustPressed(Keys.ENTER) || Gdx.input.isKeyJustPressed(Keys.BACKSPACE)) {
      if (gameState == GameState.START) {
        gameState = GameState.PLAY;
      } else {
        gameState = GameState.START;
        initBall();
     }
  }
  ```
  Once in the "PLAY" state, we start the ball’s position in the center of the screen and assign it a random starting velocity.
  Note that we changed the exit logic from `Gdx.input.isKeyPressed` to `Gdx.input.isKeyJustPressed`. 
  `isKeyJustPressed` returns true if the button has been pressed since the last call to `render`.
- Lastly, we tweak the `render() --> draw()` function so that we can see the changes from `render() --> input()` at each frame:
  ```java
  final GlyphLayout layout = new GlyphLayout(smallFont, "Hello " + gameState + " state!");
  ...
  shape.rect(ballX, ballY, 4, 4);
  ```
  The hello message now shows the game state.  
  The ball rendering now uses position now uses the variables dynamically keeping track of the ball position.

## pong-5 ("The Class Update")
- pong-5 behaves exactly like pong-4. The biggest advantage we gain from this update is in the 
  design of the code.
- Open up pong-5 to take a look at how we’ve reorganized the code using classes and objects.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/d7618d69705fa88e389b2c00d22017632d4e0851)

### Important code
- The main takeaway from this update is that we now have abstracted away from the `Main.java` the 
  logic relevant to paddle and ball mechanics. These are now in their own classes, so you’ll see a few 
  new files in the project directory. `Ball.java` contains all the logic specific to the ball, 
  while Paddle.java contains all the logic specific to each paddle. 
- This not only gives us greater flexibility moving forward, it also makes the `Main.java` file 
  cleaner and more readable.

## pong-6 ("The FPS Update")
- pong-6 adds a title to the window and displays the FPS of the application on the screen as well

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/5504dc7fa7ba87faefcda09149c3f32e9d0867e5)

### Important code
- In `Lwjgl3Launcher.java`, we set the window title with
  ```java
  configuration.setTitle("Pong");
  ```
- The second addition to the code is in the `render() --> draw()` method. 
  We display the FPS onto the screen
  ```java
  smallFont.setColor(Color.GREEN);
  final GlyphLayout fpsLayout = new GlyphLayout(smallFont, "FPS: " + Gdx.graphics.getFramesPerSecond());
  smallFont.draw(batch, fpsLayout, 10, WORLD_HEIGHT - 10);
  ```

## pong-7 ("The Collision Update")
- pong-7 allows for the Ball to bounce off the Paddles and window boundaries.
- Open up pong-7 to take a look at how we’ve incorporated AABB Collision Detection into our Pong program.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/f29d35fb6ae879ed015bcc82112662c680c7d8e7)

### AABB Collision Detection
- AABB Collision Detection relies on all colliding entities to have "axis-aligned bounding boxes", 
  which simply means their collision boxes contain no rotation in the world space, which allows us 
  to use a simple math formula to test for collision:
  ```python
  # pseudo code
  if rect1.x is not > rect2.x + rect2.width and
      rect1.x + rect1.width is not < rect2.x and
      rect1.y is not > rect2.y + rect2.height and
      rect1.y + rect1.height is not < rect2.y:
      collision is true
  else
      collision is false
  ```
  Essentially, the formula is merely checking if the two boxes are colliding in any way.
- We can use AABB Collision Detection to detect whether the Ball is colliding with the Paddles and 
  react accordingly.
- We can apply similar logic to detect if the Ball collides with a window boundary.

### Important code
- Notice how we’ve added a collides function to the `Ball` class. It uses the above algorithm to 
  determine whether there has been a collision, returning true if so and false otherwise.
- We can use this function in `render()` to keep track of the ball’s changing position and 
  velocity after each collision with a paddle:
  ```java
  if (ball.collides(player1)) {
    ball.dx = -ball.dx * 1.03f;
    ball.x = player1.x + Paddle.WIDTH;
    if (ball.dy < 0) {
      ball.dy = -randomGen.nextInt(10, 151);
    } else {
      ball.dy = randomGen.nextInt(10, 151);
    }
  }
  if (ball.collides(player2)) {
    ball.dx = -ball.dx * 1.03f;
    ball.x = player2.x - ball.width;
    if (ball.dy < 0) {
      ball.dy = -randomGen.nextInt(10, 151);
    } else {
      ball.dy = randomGen.nextInt(10, 151);
    }
  }
  ```
  Take special note of how we shift the ball away from the paddle first before reversing its 
  direction if we detect a collision in which the ball and paddle’s edges overlap! This prevents an infinite collision loop between the ball and paddle.
- We also implement similar logic for collisions with the window edges:
  ```java
  if (ball.y <= 0) {
    ball.y = 0;
    ball.dy = -ball.dy;
  }
  if (ball.y >= WORLD_HEIGHT - ball.height) {
    ball.y = WORLD_HEIGHT - ball.height;
    ball.dy = -ball.dy;
  }
  ```

## pong-8 ("The Score Update")
- pong-8 allows us to keep track of the score.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/6cffd00c3688830f4bbfb860d1092b61537eefc4) 

### Important code
- Essentially, all we need to do is increment the score variables for each player whenever 
  the ball collides with their goal boundary:
  ```java
  if (ball.x < 0) {
    servingPlayer = 1;
    player2Score++;
    ball.reset();
    gameState = GameState.START;
  } else if (ball.x >= WORLD_WIDTH - ball.width) {
    servingPlayer = 2;
    player1Score++;
    ball.reset();
    gameState = GameState.START;
  }
  ```

## pong-9 ("The Serve Update")
- pong-9 introduces a new state, "SERVE", to our game.

[Diff](https://github.com/cyrilou242/cs50-pong-java-libgdx/commit/7e55fe6744531e2a34888c3150d08c75595671d6)

### What is a State Machine?
- Currently in our Pong program we’ve only talked about state a little bit. We have the "START" 
  state, which means the game is ready for us to press "ENTER" so that the ball will start 
  moving, and the "PLAY" state, which means the game is currently underway.
- A state machine concerns itself with monitoring what is the current state and what transitions 
  take place between possible states, such that each individual state is produced by a specific 
  transition and has its own logic.
- In pong-9, we allow a player to "SERVE" the ball by not having to defend during their first turn.
- We transition from the "PLAY" state to the "SERVE" state by scoring, and from the "SERVE" state 
  to the "PLAY" state by pressing enter. The game begins in the "START" state, and transitions to 
  the "SERVE" state by pressing enter.

### Important Code
- We can add the new "serve" state by making an additional condition within the `render` function:
  ```java
  if (gameState == GameState.SERVE) {
    ball.dy = randomGen.nextInt(0, 101) - 50;
    if (servingPlayer == 1) {
      ball.dx = randomGen.nextInt(140, 200) ;
    } else {
      ball.dx = -randomGen.nextInt(140, 200);
    }
  }
  ```
  The idea is that when a player gets scored on, they should get to serve the ball, 
  so as to not be immediately on defense. We do this by adjusting the ball velocity in 
  the "SERVE" state based off which player is serving.

## pong-10 (“The Victory Update”)
coming soon
  
   
  


