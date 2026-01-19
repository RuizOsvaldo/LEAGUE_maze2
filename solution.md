let hasKey = false
let key: Sprite = null
let obstacle: Sprite = null

scene.setBackgroundColor(7)

let player = sprites.create(assets.image`guy`, SpriteKind.Player)
controller.moveSprite(player)
player.setStayInScreen(true)

let door = sprites.create(assets.image`door`, SpriteKind.Projectile)
door.setPosition(150, 60)

key = sprites.create(assets.image`coin`, SpriteKind.Food)
key.setPosition(20, 60)

info.startCountdown(20)

game.onUpdateInterval(1000, function () {
    obstacle = sprites.create(assets.image`Obstacle`, SpriteKind.Enemy)
    obstacle.setPosition(Math.randomRange(10, 150), 0)
    obstacle.vy = Math.randomRange(30, 60)
    obstacle.setFlag(SpriteFlag.AutoDestroy, true)
})

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

sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    if (hasKey) {
        otherSprite.destroy()
        game.over(true, effects.starField)
    } else {
        game.splash("You need the key!")
    }
})

sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    otherSprite.destroy(effects.disintegrate, 200)
    info.changeLifeBy(-1)
    if (info.life() <= 0) {
        game.over(false, effects.melt)
    }
})