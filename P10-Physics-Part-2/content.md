---
title: Penguin launcher
slug: physics-penguin
---

Ready to try out the catapult on the penguins?

You already know how to dynamically add a new penguin to the scene, this time when 
you add a new penguin you're going to pin joint it to the catapult arm and then release 
it when the player lets go of the catapult arm and watch physics in action. Let's add 
a new *penguinJoint* for this purpose.

# Pinning the penguin

> [action]
> Add this property to your *GameScene* class:
>
```
var penguinJoint: SKPhysicsJointPin?
```
>

When the player touch begins, you want to spawn a penguin and then pin it to the bucket 
area of the catapult arm.

> [action]
> Add this code to the `touchesBegan(_ touches: with event:)` method immediately after 
> the **touchJoint** but still inside the if statement block. 
> code:
>
```
let penguin = Penguin()
addChild(penguin)
penguin.position.x += catapultArm.position.x + 20
penguin.position.y += catapultArm.position.y + 50
penguin.physicsBody?.usesPreciseCollisionDetection = true
penguinJoint = SKPhysicsJointPin.joint(withBodyA: catapultArm.physicsBody!,
                                       bodyB: penguin.physicsBody!,
                                       anchor: penguin.position)
physicsWorld.add(penguinJoint!)
cameraTarget = penguin
```
>

The code is very similar to the original beta launcher. The manual impulse has been 
removed and replaced with create a pin joint between the penguin and the catapult arm. 
This joint will be maintained until the player releases their touch.

## Let it go

To get the penguin to fly we need to remove the *penguinJoint* when the player 
releases their touch.

To make the system work more reliably you will give the penguin an impulse at a vector
that is perpendicular to the catapult arm. 

> [solution]
> Add the following to the end of `touchesEnded(_ touches: with event:)` method:
>
```
// Check for a touchJoint then remove it. 
if let touchJoint = touchJoint {
    physicsWorld.remove(touchJoint)
}
// Check for a penguin joint then remove it. 
if let penguinJoint = penguinJoint {
    physicsWorld.remove(penguinJoint)
}
// Check if there is a penuin assigned to the cameraTarget
guard let penguin = cameraTarget else {
    return
}
// Generate a vector and a force based on the angle of the arm.
let force: CGFloat = 350
let r = catapultArm.zRotation
let dx = cos(r) * force
let dy = sin(r) * force
// Apply an impulse at the vector. 
let v = CGVector(dx: dx, dy: dy)
penguin.physicsBody?.applyImpulse(v)
```
>

You can give this a test in the simulator. There is still more work to do but at this point
the core game mechanic should be functioning. Be aware that dragging the arm too far back 
may throw the penguin way over the top, and after the penguin flies you are not able to 
get back to the camera unless you press reset. 

# Summary

Well done! You have the first fully playable prototype build.

You've learnt how to:

- Launch a physics body indirectly using your catapult
- Tweaking physics to improve the simulation

In the next chapter you will be working with physics contacts and collisions.
