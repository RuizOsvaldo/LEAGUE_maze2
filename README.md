 # Advanced Key Maze

## Introduction @unplugged
Welcome to The LEAGUE's Intermediate Key Maze game! This challenging game combines everything you've learned: collecting items, avoiding obstacles, managing lives, and racing against time. Navigate through falling obstacles, find the key, and reach the door before time runs out. Are you ready for the challenge?

## Step 1

Set up the game world with a background color.

From ``||scene:Scene||``, use ``||scene:set background color||`` to choose your game's theme.

```blocks
scene.setBackgroundColor(7)
```

## Step 2

Create the player character who will navigate the maze.

From ``||sprites:Sprites||``, ``||sprites:create a sprite||`` named **player** with kind **Player** using the `guy` image from your assets.

```blocks
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
```

## Step 3

Add movement controls so the player can navigate.

From ``||controller:Controller||``, use ``||controller:move sprite with buttons||`` for the player sprite.

```blocks
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
```

## Step 4

Keep the player within the screen boundaries.

From ``||sprites:Sprites||``, ``||sprites:set stay in screen||`` to **ON** for the player.

```blocks
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)
```

## Step 5

Create the door that serves as the exit goal.

``||sprites:Create a sprite||`` named **door** with kind **Projectile** using the `door` image. Set its position to **x: 150, y: 60** (far right side).

```blocks
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)
let door = sprites.create(assets.image`door`, SpriteKind.Projectile)
door.setPosition(150, 60)
```

## Step 6

Create the key that the player must collect.

``||sprites:Create a sprite||`` named **key** with kind **Food** using the `coin` image. Set its position to **x: 20, y: 60** (far left side).

```blocks
let key: Sprite = null
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)
let door = sprites.create(assets.image`door`, SpriteKind.Projectile)
door.setPosition(150, 60)
key = sprites.create(assets.image`coin`, SpriteKind.Food)
key.setPosition(20, 60)
```

## Step 7

Add a countdown timer to increase the challenge!

From ``||info:Info||``, use ``||info:start countdown||`` and set it to **20** seconds. Players must complete the challenge before time runs out!

```blocks
let key: Sprite = null
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)
let door = sprites.create(assets.image`door`, SpriteKind.Projectile)
door.setPosition(150, 60)
key = sprites.create(assets.image`coin`, SpriteKind.Food)
key.setPosition(20, 60)
info.startCountdown(20)
```

## Step 8

Create a variable to track if the player has collected the key.

From ``||variables:Variables||``, create a variable called **hasKey** and ``||variables:set hasKey to false||`` at the start.

```blocks
let hasKey = false
let key: Sprite = null
scene.setBackgroundColor(7)
let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)
let door = sprites.create(assets.image`door`, SpriteKind.Projectile)
door.setPosition(150, 60)
key = sprites.create(assets.image`coin`, SpriteKind.Food)
key.setPosition(20, 60)
info.startCountdown(20)
```

## Step 9

Now let's spawn falling obstacles to dodge! This makes the game much harder.

From ``||game:Game||``, use ``||game:on game update every 1000 ms||`` to create obstacles every second.

```blocks
let obstacle: Sprite = null
game.onUpdateInterval(1000, function () {
	
})
```

## Step 10

Inside the interval, create falling obstacle sprites.

``||sprites:Create a sprite||`` named **obstacle** with kind **Enemy** using the `Obstacle` image.

```blocks
let obstacle: Sprite = null
game.onUpdateInterval(1000, function () {
    obstacle = sprites.create(assets.image`Obstacle`, SpriteKind.Enemy)
})
```

## Step 11

Make obstacles spawn at random positions at the top of the screen.

Use ``||sprites:set position||`` with:
- x: ``||math:pick random||`` from **10** to **150**
- y: **0** (top of screen)

```blocks
let obstacle: Sprite = null
game.onUpdateInterval(1000, function () {
    obstacle = sprites.create(assets.image`Obstacle`, SpriteKind.Enemy)
    obstacle.setPosition(Math.randomRange(10, 150), 0)
})
```

## Step 12

Make obstacles fall down at random speeds!

Set ``||sprites:vy (vertical velocity)||`` to ``||math:pick random||`` from **30** to **60**. Also set ``||sprites:auto destroy||`` to **ON** so obstacles disappear when they leave the screen.

```blocks
let obstacle: Sprite = null
game.onUpdateInterval(1000, function () {
    obstacle = sprites.create(assets.image`Obstacle`, SpriteKind.Enemy)
    obstacle.setPosition(Math.randomRange(10, 150), 0)
    obstacle.vy = Math.randomRange(30, 60)
    obstacle.setFlag(SpriteFlag.AutoDestroy, true)
})
```

## Step 13

Handle what happens when the player collects the key.

Use ``||sprites:on sprite overlaps||`` for **Player** and **Food**. When they overlap:
- ``||sprites:destroy||`` the key
- ``||variables:set hasKey to true||``
- ``||game:splash||`` "You got the key!"

```blocks
let hasKey = false
sprites.onOverlap(SpriteKind.Player, SpriteKind.Food, function (sprite, otherSprite) {
    otherSprite.destroy()
    hasKey = true
    game.splash("You got the key!")
})
```

## Step 14

Add a bonus reward system when collecting the key! (Advanced feature)

After the splash message, add an ``||logic:if-else||`` block using ``||math:percent chance 50||``:
- **If true**: ``||info:change score by 5||`` and splash "Bonus 5 points!"
- **Else**: Start ``||game:confetti effect||`` for **500** ms

```blocks
let hasKey = false
sprites.onOverlap(SpriteKind.Player, SpriteKind.Food, function (sprite, otherSprite) {
    otherSprite.destroy()
    hasKey = true
    game.splash("You got the key!")
    if (Math.percentChance(50)) {
        info.changeScoreBy(5)
        game.splash("Bonus 5 points!")
    } else {
        effects.confetti.startScreenEffect(500)
    }
})
```

## Step 15

Make the key respawn at a new random location after collection! (Optional advanced feature)

After the bonus logic, recreate the key sprite with a new random position from **10 to 150** (x) and **10 to 110** (y).

```blocks
let key: Sprite = null
let hasKey = false
sprites.onOverlap(SpriteKind.Player, SpriteKind.Food, function (sprite, otherSprite) {
    otherSprite.destroy()
    hasKey = true
    game.splash("You got the key!")
    if (Math.percentChance(50)) {
        info.changeScoreBy(5)
        game.splash("Bonus 5 points!")
    } else {
        effects.confetti.startScreenEffect(500)
    }
    key = sprites.create(img`
        . . 5 5 5 . . 
        . 5 7 7 7 5 . 
        5 7 7 7 7 7 5 
        5 7 7 7 7 7 5 
        5 7 7 7 7 7 5 
        . 5 7 7 7 5 . 
        . . 5 5 5 . . 
        `, SpriteKind.Food)
    key.setPosition(Math.randomRange(10, 150), Math.randomRange(10, 110))
})
```

## Step 16

Handle reaching the door - but only if you have the key!

Use ``||sprites:on sprite overlaps||`` for **Player** and **Projectile**. Add an ``||logic:if-else||`` checking **hasKey**:
- **If true**: Destroy the door and ``||game:game over WIN||`` with **starField** effect
- **Else**: ``||game:splash||`` "You need the key!"

```blocks
let hasKey = false
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    if (hasKey) {
        otherSprite.destroy()
        game.over(true, effects.starField)
    } else {
        game.splash("You need the key!")
    }
})
```

## Step 17

Finally, handle collisions with falling obstacles!

Use ``||sprites:on sprite overlaps||`` for **Player** and **Enemy**. When they collide:
- Destroy the obstacle with ``||sprites:disintegrate effect||``
- ``||info:change life by -1||``
- Check ``||logic:if||`` life <= 0, then ``||game:game over LOSE||`` with **melt** effect

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    otherSprite.destroy(effects.disintegrate, 200)
    info.changeLifeBy(-1)
    if (info.life() <= 0) {
        game.over(false, effects.melt)
    }
})
```

## Conclusion @unplugged

Incredible work! You've created an advanced game with multiple systems working together!

**Advanced concepts you mastered:**
- Moving sprites with velocity
- Life management system
- Random chance mechanics (bonus rewards)
- Respawning collectibles
- Multiple win/lose conditions
- Combining timers with obstacles

**Challenge ideas:**
- Increase obstacle spawn rate over time
- Add power-ups that give temporary invincibility
- Create multiple keys to collect
- Add different types of obstacles with different behaviors
- Make the key required to unlock the door (no respawn)
- Add sound effects for different events

You're now ready to create complex games with multiple mechanics!


> Open this page at [https://ruizosvaldo.github.io/league_maze2/](https://ruizosvaldo.github.io/league_maze2/)

## Use as Extension

This repository can be added as an **extension** in MakeCode.

* open [https://arcade.makecode.com/](https://arcade.makecode.com/)
* click on **New Project**
* click on **Extensions** under the gearwheel menu
* search for **https://github.com/ruizosvaldo/league_maze2** and import

## Edit this project

To edit this repository in MakeCode.

* open [https://arcade.makecode.com/](https://arcade.makecode.com/)
* click on **Import** then click on **Import URL**
* paste **https://github.com/ruizosvaldo/league_maze2** and click import

#### Metadata (used for search, rendering)

* for PXT/arcade
<script src="https://makecode.com/gh-pages-embed.js"></script><script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>
