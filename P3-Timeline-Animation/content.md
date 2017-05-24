---
title: Animate a character using the timeline
slug: animating-spritekit
---

Let's learn how to create animations with SpriteKit's timeline. You are going to add a 
taunting animation to the bear that will sit behind our catapult:

![Bear preview](../Tutorial-Images/bear_inaction_preview.png)

# Build a bear

The bear will have it's own `.sks` file.

> [action]
> Create a new file called *Bear.sks* (`File > New > File`) in your project:
>
> ![Creating the SKS File](../Tutorial-Images/p3-01-new-sks.png)

You need to combine two images to make the bear, the body without an arm and a separate 
arm that will then be animated.

> [action]
> Resize the content of this view. Set the Height and width of the Bear scene to 
> 80 x 80.
>
> ![Resize Bear](../Tutorial-Images/p3-02-resize-bear-scene.png)
>

Show the Media Library in the lower right by clicking the small Film icon. 

![Media Library](../Tutorial-Images/p3-02-media-library.png)

> [action]
> Select *Bear.sks* and `Zoom Out` the scene so you can see the white border.
> Drag *bearnoarms.png* in from your *Media Library* to the scene, snap it to snap the 
> bottom left corner of the scene.
>
> ![Adding the bear asset](../Tutorial-Images/p3-03-bear-no-arm.png)
>

> [info]
> The Bear doesn't snap to the lower corner choose Editor > Snap to Nodes. 
>

> [action]
> Add *beararm.png* and set the *Anchor Point* to `(0,1)`, position it somewhere that looks sensible.
> Set the *Z* to `1` to ensure the arm is always on top of the body.
>
> ![Adding the bear arm asset](../Tutorial-Images/p3-04-bear-arm-settings.png)

When you apply rotation to a *SKNode* it will rotate around the anchor point - for the arm, this 
should ideally be somewhere near the shoulder, which in this case is the top left corner 
of the image.

# Adding animation

Now you can animate the polar bear's arm. In SpriteKit there is a default **Timeline** for adding animation actions.  However, you are going to use a more powerful feature and create shared animations, which are stored in a *SpriteKit Action* file, this gives you the power to reuse the custom animations you create with any node.

> [action]
> Open the timeline then click on *+* to add a new timeline and name it `ArmAnimation`, 
> when prompted to create a new file name it `CharacterActions`.
>
> First open the timeline at the bottom of the editor.  
>
> ![Adding the character action file](../Tutorial-Images/p3-05-timeline.png)
>
> Next create a new action by clicking the **+**, choosing *Create new file*, last set the name to *CharacterActions*. 
>
> ![Create ArmAnimation action](../Tutorial-Images/p3-06-create-action.png)
> 
> Last save the new actions. 
>
> ![CharacterAction](../Tutorial-Images/p3-07-character-actions.png)
>

## Adding actions to the timeline

You will be animating the arm using two rotation actions, the first one will *rotate* the 
arm by `90` degrees and the second will *rotate* it back by `90` degrees. 

> [action]
> You should be in *CharacterActions.sks*, expand *ArmAnimation* and locate the *Rotate*
> action in the *Object library*.
> Drag this into the timeline, the default values of *Duration* `1` and *Degrees* `90` 
> works well.
> Drag another rotation in and set the *Duration* to `1` and the *Degrees* to `-90`
>
> ![Define the arm animation actions](../Tutorial-Images/p3-08-add-rotation.png)
>

Congratulations! Let's try this out on the Bear.

## Applying custom actions

> [action]
> Open *Bear.sks*, have a look in your *Object Library* you should see your custom action 
> `ArmAnimation`.
> Drag this into the timeline for the arm, ensure the duration is `00:02` by dragging the 
> edge of the action bar or set *Duration* to `2`.
> 
> **NOTE!** you may not see the animation appear on the timeline if the duration, in 
> the upper right, is 0! Setting the duration to a value greater than 0 will make the 
> animation appear on the timeline. 
>
> ![Add custom arm animation action to timeline](../Tutorial-Images/p3-09-add-rotation.png)
>
> Next you will want to ensure this animation loops forever, click on the **loop** symbol 
> in the bottom-left of your **ArmAnimation** action and highlight the infinity symbol.
>
> ![Loop arm animation action](../Tutorial-Images/p3-10-loop-arm-animation.png)
>

Great job, time to press *Animate* at the top of the timeline editor and your bear should 
hopefully look like this:

![Press animate](../Tutorial-Images/p3-11-bear-arm-animation-1.png)

![Bear arm animation](../Tutorial-Images/p3-11-bear-arm-animation.gif)

# Summary

You've learnt to:

- Create a new SpriteKit Scene for the bear
- Create a custom `ArmAnimation` action in a shared SpriteKit Action file
- Apply your custom action to the bear timeline

In the next chapter you will be creating more game objects, ready for use in the game.
