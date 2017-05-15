---
title: Particle Effects
slug: particle-effect
---

Now you are going to add an Angry Bird style particle effect whenever a seal gets 
eliminated. SpriteKit has a great integrated particle effect designer which we are 
going to use to define the style of our first particle effect.

# Creating a new particle effect

> [action]
> Create a new particle effect `File > New > File > SpriteKit Particle File`
>
> ![SpriteKit Particle File](../Tutorial-Images/p12-01-particle-emitter.png)
>
> Choose Smoke from the menu. This is one of the default settings. 
> 
> ![SpriteKit Particle File](../Tutorial-Images/p12-02-smoke.png)
>
> Save this with the name "Poof"
>

Select "Poof" in the project outline on the left. It should look something like this.

![SpriteKit Particle Black Smoke](../Tutorial-Images/animated_black_smoke.gif)

I ended up making a more subtle smoke effect. You can copy the property values shown 
or feel free to go crazy and create your own unique look.

> [action]
> Particle attributes:
>
> ![Particle Attributes 1](../Tutorial-Images/p12-03particle-settings.png)
>
> You can use the color ramp to create interesting cycles of color.
>
> ![Particle Attributes 2](../Tutorial-Images/xcode_spritekit_particle_2.png)
>

This should look something like that:

![SpriteKit Particle Grey Smoke](../Tutorial-Images/animated_grey_smoke.gif)

Feel free to spend some time playing around with different values.

## Adding the particle effect to our collision

Let's add some code that adds the particle effect to the scene whenever a seal gets 
eliminated.

> [action]
> Add the following code to the start of the `dieSeal` method:
>
```
/* Load our particle effect */
let particles = SKEmitterNode(fileNamed: "Poof")!
/* Position particles at the Seal node */
particles.position = node.position
/* Add particles to scene */
addChild(particles)
let wait = SKAction.wait(forDuration: 5)
let removeParticles = SKAction.removeFromParent()
let seq = SKAction.sequence([wait, removeParticles])
particles.run(seq)
```
>

Here you create an SKEmitter instance using the "Poof" particle you configured. Then 
add it as a child of this scene, and position it at the same position as the Seal. 
Then you set up an action that will remove the `particles` after 5 seconds. 

Note that I set the *Emitter Maximum* to 60 this limits the number of particles produced. 
The wait time needs to be long enough so the emitter can run through the entire life of 
the 60 particles it will emit, then we can remove it. 

# Summary

Well done! Now you know how to create particle effects and make them play when certain 
events in your game occur.

In the next chapter you will be adding animated penguins to the sidelines.
