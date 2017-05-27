---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the flying penguin, but 
won't scroll back to the catapult when the attempt is completed. You are going to that 
in this chapter.

# Reset the camera

When a Penguin flies off the screen or comes to rest on the screen we need to move the camera back over 
to the catapult to be ready to launch the next Penguin. It's good to break our code into functions that 
we can reuse so you will write a function to handle this. 

The first job is to write a function to animate the camera back over to the catapult. 

> [action]
> Add a new function to the `GameScene` class:
>
```
func resetCamera() {
    /* Reset camera */
    let cameraReset = SKAction.move(to: CGPoint(x:0, y:camera!.position.y), duration: 1.5)
    let cameraDelay = SKAction.wait(forDuration: 0.5)
    let cameraSequence = SKAction.sequence([cameraDelay,cameraReset])
    cameraNode.run(cameraSequence)
    cameraTarget = nil
}
```
>

This function creates a move action that moves the camera to (x: 0, y: 0). This should be 
center of the left half of the screen. That is, if the anchor point of the screen is in the center. 
If not you can check the initial location of the camera and use those coordinates.

Next we define a wait action. 

Then there is sequence action. This combines the wait and the move. 

Last we run the sequence on the camera. So the effect, when you call `resetCamera()` is wait
half a second, then move back to the starting position. 

At the very end we set the `cameraTarget` is set to `nil` since we aren't following that object 
any more. 

# Checking on the penguin

If a penguin has stopped or is moving slowly we will remove `cameraTarget` and move the 
camera back to the catapult and reset the catapult arm itself.

Also if a penguin falls below the edge of the screen it has fallen out of the world
and should be removed, and the camera reset. 

# Checking the speed

To check the speed of the penguin we need to look at it's motion in both the x and y. 
The speed is a combination of both. The penguin has a velocity x and y. If we picture 
this as a right triangle the distance moved is the length of the hypotenuse.

![triangle-math](../Tutorial-Images/p12-11-triangle-math.png)

Using the Pythagorean Theorem you can find the length. 

`length = sqrt((x*x)+(y*y))`

We can make this more conveneient to calculate by writing an extension to the CGVector
class. 

> [action]
> Add the following *outside* the GameScene class. You can put it at the bottom of the 
> file where you defined `clamp` earlier. 
> 
```
extension CGVector {
    public func length() -> CGFloat {
        return CGFloat(sqrt(dx*dx + dy*dy))
    }
}
```
>
> The extension adds a new method to CGVector: `length()`. This method returns a CGFloat 
> which is the length of the vector.
> 

We also need to check if the peguin has left the visible area of the game. In out case 
it's safe to say that if the peguin fall below the ground we will never see it again. 
This translates to roughly y = -200.

> [action] 
> Write a function to check on a flying penguin. 
> 
```
func checkPenguin() {
    guard let cameraTarget = cameraTarget else {
        return
    }
    
    /* Check penguin has come to rest */
    if cameraTarget.physicsBody!.joints.count == 0 && cameraTarget.physicsBody!.velocity.length() < 0.18 {
        resetCamera()
    }
    
    if cameraTarget.position.y < -200 {
        cameraTarget.removeFromParent()
        resetCamera()
    }
}
```
>

This function first checks if the `cameraTarget` is not `nil` with guard. Remember `cameraTarget` 
is the penguin you just launched, and it is optional. There may not be a peguin in which case 
there is nothing to check. 

If there is a `cameraTarget` first we check the number of `joints` it's `physicsBody`. If there 
is a joint the penguin is attached to the catapult arm, in which case it might be moving slowly
as you pull the arm back and we don't want to launch it. We also check the speed by getting the 
length of the `velocity` vector here, if the length is less than `0.18` we call `resetCamera()`.

Next you check the `position.y` of the cameraTarget/penguin. If the y is less than (`<`) -200
the penguin is below the ground and out of view so we remove it and reset the camera. 

## Final tweak

Last hide the red helper sprites: 'cantileverNode' and 'touchNode' by moving them behind
the background. You can do this by giving them a Z-Position of `-3`. 

# Summary

You've learned the importance of making those **little touches**, it's often the small 
things when added together that can really take your game play to the next level and 
make the experience more memorable for the player.

Always look for ways to improve your game mechanic and make it feel as satisfying as 
possible for the player. Get feedback and iterate.

In the next chapter where you will learn how you can make your game iPad compatible!
