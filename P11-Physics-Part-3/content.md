---
title: Working with contacts and collisions
slug: physics-contact-collisions
---

Next you'll learn how to setup the physics body *Contact Masks* and use that knowledge
to remove seals when they are hit by penguins, ice blocks or even each other.

Before we continue let's clarify some terminology.

**Categories**

Categories refer to different groups of objects. In your game Penguins, Ice Blocks,
and Seals would be different categories. We need to know when penguins and Ice Blocks
hit Seals.

**Contact**

Contact refers to two physics objects making contact. A contact does NOT represent a
physical interaction! In other words you can create two objects that pass through
each other but produce a contact when they overlap.

We might want to know when Penguins, or Ice Blocks contact a Seal.

**Collision**

Collision refers to a physical interaction between objects. Objects that show physical
interactions stop, bounce, and knock each other around.

Our game needs collisions between Penguins, Ice Blocks, Ground, Seals, and Catapult Arm.

# Physics masks

Bit masks are numbers where each bit in the number acts as a switch. Computers keep
track of numbers using bits. An 8 bit number is made up 8 bits. Which you can picture as:

- `00000000`

That would be 0.

- `00000000 = 0`
- `00000001 = 1`
- `00000010 = 2`
- `00000011 = 3`
- `00000100 = 4`
- `00000101 = 5`

As 32 bit numbers they would look like this:

- `0 = 00000000 00000000 00000000 00000000`
- `1 = 00000000 00000000 00000000 00000001`
- `2 = 00000000 00000000 00000000 00000010`
- `3 = 00000000 00000000 00000000 00000011`
- `4 = 00000000 00000000 00000000 00000100`
- `5 = 00000000 00000000 00000000 00000101`
- `6 = 00000000 00000000 00000000 00000110`
- `7 = 00000000 00000000 00000000 00000111`
- `8 = 00000000 00000000 00000000 00001000`

The physics system used in SpriteKit uses 32 bit numbers to manage physics categories,
contacts, and collisions. This allows you to create many different kinds of interactions
between objects in the games you create.

# Category Bitmask

Each *physicsBody* has a property called *categoryBitMask*. This *categoryBitMask* is used
to identify different categories of physics objects. Categories might be things like:

- `00000000 = 0` None - use this for objects that do not interact with other objects
- `00000001 = 1` Penguin - use this for penguins we fling
- `00000010 = 2` Seal - Use this for Seals
- `00000100 = 4` Ice Block - Use this Ice blocks
- `00001000 = 8` Ground - This identifies the ground

Categories should usually be numbers with only a single 1 bit! Notice each of the numebrs
above are 1, 2, 4, 8. We skipped 3 (00000011) which would represent the same category as
Penguin 1 (00000001) and Seal 2 (00000010).

## Contact Bitmask

A contact bit mask says which categories produce a message when they make contact. For
example an object with a `contactBitmask` of 3 (00000011) would produce a message when
it hit an object with a category of 1 (00000001) or 2 (00000010).

- `00000001`
- `00000010`
- `00000011` <- Shares the first and second bit with 1 and 2.

## Collision Bitmask

The `collisionBitmask` determines which objects show physical interaction. Set the bits
to 1 for each category of object that **collides** with another object. For example
Penguins collide with Seals, Ice Blocks, and the Ground.

- `000000001 = 1` = Penguin category
- `000000010 = 2` = Seal category
- `000000100 = 4` = Ice Block category
- `000001000 = 8` = Ground category
- `000001110 = 14` = penguin collision bitmask

Penguin collision = Seal + Ice Block + Ground = 2 + 4 + 8 = 14

Notice that Penguins don't collide with other Penguins! If Penguins need to collide with
other Penguins their `collisionBitMask` would be:

Penguin collision = Penguin + Seal + Ice Block + Ground = 1 + 2 + 4 + 8 = 15

## Bitwise operations

Bitwise operations are math operations that applied at the binary, or bit, level. There are two operations that we need to work with.

### Bitwise AND &

When combing two binary numbers with a bitwise AND we compare each bit if BOTH bits are 1,
then that bit in the answer is 1. Think about it like this. For each bit in the same
column, if the bit on top is 1 AND the bit on the bottom is 1, then that bit in the answer
is 1. Here is an example:

```
  00000000
& 00000010
= 00000000

  00000011
& 00000010
= 00000010

  00000011
& 00000010
= 00000010
```

### Bitwise OR |

A bitwise OR operation compares the bit in each column. If the bit in either column is 1
then the answer is 1. Here is an example:

```
  00000000
| 00000010
= 00000010

  00000011
| 00000010
= 00000011

  00000011
| 00000100
= 00000111
```

You remember that you setup the penguin with a **Category Mask** of `1` and the seal
with `2`. When a collision takes place you will be checking the *categoryBitMask* for a
value of `2` to identify when a seal has been involved in a collision.

By default SpriteKit will not inform you of any collisions, I'm sure you've spotted the
*Contact Mask* property when setting up physics bodies.  By default they are all `0` so
our *contactDelegate* will never be informed of any collisions taking place.

This use case is pretty straightforward, if **anything** collides with a seal (even seal
on seal) you want to be informed.

## Set the contactBitmask and categoryBitMask

> [action]
> Open your level and click each seal, set `Contact Mask` to `1`.

Now anytime a seal is involved in a collision you will be informed through the contact
delegate.

> [action]
> Now set each seal's `categoryBitmask` to `2`.

Now we can identify Seals during a collision via the value of their categoryBitMask.

> [info]
>
> You need to set these settings for all Seals! Can you think of a better way we could have set up the Seals? Maybe `.sks` files...😉

# The Physics contact delegate

While playing the game contact between some of the objects is meaningful. For example
when a Seal hits the ground, an Ice Block, or a Penguin we want to know. If the collision is hard
enough we want to remove the Seal.

A 'contact' is a message letting us know that two objects made contact. The physics engine manifests
a contact by calling the `didBegin(contact:)` method on it's physics contact delegate.

A delegate is defined as:

1. Entrust (a task or responsibility) to another person.
1. A person sent or authorized to represent others.

The physics contact delegate is the object you want to to handle phsyics contact events when they occur.
Usually this will be the scene object.

In order for an object to be a delegate it must conform to a protocol -- the official procedure or system of rules.

A protocol is a "system of rules". In Swift when a class declares a protocol it is an agreement that the
class defines the methods and properties declared by the protocol.

To make this work you need to declare your `GameScene` class as conforming to the `PhysicsContactDelegate`
protocol. Set the class as the `physicsContactDelegate`, and last define the `didbegin(contact:)` method.

> [action]
> Modify the GameScene class decleration implement the *SKPhysicsContactDelegate*:
>
```
class GameScene: SKScene, SKPhysicsContactDelegate {
```
>

Now that you've adopted the contact delegate protocol, you need to sign up to receive
events generated by the contact delegate.

> [action]
> Add this code to `didMove(to view:)`:
>
```
/* Set physics contact delegate */
physicsWorld.contactDelegate = self
```
>

# Implementing a delegate method

You will need to implement the *beginContact(...)* delegate method that will inform you of any valid collision contacts.

> [action]
> Add this method to the *GameScene* class:
>
```
func didBegin(_ contact: SKPhysicsContact) {
    /* Physics contact delegate implementation */
    /* Get references to the bodies involved in the collision */
    let contactA:SKPhysicsBody = contact.bodyA
    let contactB:SKPhysicsBody = contact.bodyB
    /* Get references to the physics body parent SKSpriteNode */
    let nodeA = contactA.node as! SKSpriteNode
    let nodeB = contactB.node as! SKSpriteNode
    /* Check if either physics bodies was a seal */
    if contactA.categoryBitMask == 2 || contactB.categoryBitMask == 2 {
       print("Seal Hit")
    }
}
```
>

Run your game...

You should see the log message appear in the console every time a seal is hit.  You may
notice you a few seal hits at the start, currently this code is super sensitive, the act
of adding *Level1.sks* to the scene will mostly generate a contact event as the seal
gently touches the ground when they are first added to the scene.

## Removing the seal

First you retrieve the collision impulse between the seal and the other object. If this
impulse is large enough you decide to remove the seal by using the *removeSeal* method.

> [action]
> Add the *removeSeal* method to your *GameScene* class:
>
```
func removeSeal(node: SKNode) {
   /* Seal death*/
>
   /* Create our hero death action */
   let sealDeath = SKAction.run({
        /* Remove seal node from scene */
        node.removeFromParent()
    })
    self.run(sealDeath)
}
```
>

# Destroying Seals

Now that you know that the collision handler is working, let's implement the real
functionality. What you want is a way to check how hard the seal was hit and use that to
decided if you should ignore the collision or destroy the seal.  

SpriteKit to the rescue, you can utilize the *collisionImpulse* property of the collision
to gauge the resulting impact force from the collision.

To keep things clean let's setup a separate seal removal method. You'll be adding more
functionality to this method later.

> [action]
> Replace:
>
```
print("Seal Hit")
```
>
> with:
>
```
/* Was the collision more than a gentle nudge? */
if contact.collisionImpulse > 2.0 {
    /* Kill Seal */
    if contactA.categoryBitMask == 2 { removeSeal(node: nodeA) }
    if contactB.categoryBitMask == 2 { removeSeal(node: nodeB) }
}
```
>

This code should be fairly familiar, yet one does not simply call`removeFromParent()`
directly...

In the physics simulation you need to ensure that the seal is removed at the right time,
in the SpriteKit frame render cycle there is a **post-physics** step called
*didSimulatePhysics* after the physics simulation is complete for the current frame.
At this point it's safe to remove a physics body, otherwise you can run into problems
with triggering collisions on bodies that have been removed, resulting in a horrible
game crash.  If you wrap this in an *SKAction*, SpriteKit will ensure it's executed at
the right time in the render cycle.

# Summary

The game mecanic is nearly finished, you've learned to:

- Implement the physics *SKPhysicsContactDelegate*
- Using the **categoryBitMask** to identify physics bodies in a collision
- Using **SKPhysicsContact** to evaluate the collision force between two bodies
- Removing the seal cleanly from the scene

In the next chapter it's time to add a little polish.
