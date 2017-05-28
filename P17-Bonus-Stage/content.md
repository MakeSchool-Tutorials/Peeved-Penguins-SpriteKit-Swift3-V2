---
title: Welcome to bonus stage
slug: bonus-stage
---

Welcome to bonus stage, your challenge should you accept, is to work through the list of 
challenges and take your game to the next level.

There are no solutions provided and I would encourage you to discuss potential solutions 
with your fellow students, it's good to collaborate and often results in new exciting 
directions.

If you have any challenge suggestions, we would love to hear them.  Please feedback at 
the [GitHub Repository](https://github.com/MakeSchool-Tutorials/Peeved-Penguins-SpriteKit-Swift)

It would be a good idea to create a **new branch** in your repo for these challenges.

# The challenge

The current game has no lives, you have infinite penguin ammo. There is no challenge...

> [challenge]
> Implement a game lives system? It might be nice to use the jumping penguins as the 
> representation of these lives.

# Scoring

There is no scoring element!

> [challenge]
> Add a scoring mechanism, you could assign score values to different objects. What 
> about a high score?

# Penguin properties

You have one type of penguin with fixed physics properties, how about a variety of 
penguins?

> [challenge]
> Can you create different types of penguins with different characteristics?
> **Hint**: You don't need extra GFX, you can always change the color of a sprite, remember you are prototyping ideas.

# More levels

What happens when you kill all of the seals? Nothing... Let's implement a level mechanic 
and build some new level content.

> [challenge]
> Implement a level mechanic and add at least 1 new level!

You can copy level_1.sks and rename it Level_2.sks. To load a level call:

- `GameScene.level(1)` Loads level 1
- `GameScene.level(2)` Loads level 2

Here are some ideas. 

- Make new levels by copying *Level_1.sks*. You can change the background and create new arrangements of seals 
and ice blocks. You can also change the background. You **must** keep the main game objects: Catapult, Catapult 
arm, Ground, Reset button. 
- Make a button for each level in the `MainMenu` scene. Set the action for these buttons to load different levels. 
Make a level for each button. You'll need to be able to get back to the Main Menu from Game Scene. A button would 
make this easy. 
- Count the number of seals in a level when the level loads. You can count the seals that are destroyed 
in the `removeSeal()` method. When the count gets to 0 you can load the next level. Using a class property to 
keep track of the current level would make this easy. 
- Combine the two ideas above. You can disable buttons for levels that are not available. `MSButtonNode` has a 
`state` property, one of the enum values is `disabled`. You can check the current level and enable buttons to 
make the appropriate levels enabled. 

# Catapult physics

The catapult physics could be improved. The big problem with the physics of the game is the world is too 
small in relation to the size of the catapult. To make this play better you can increase the size of the 
world.

> [challenge]
> Can you create an improved physics shape in code to replace the catapult arm alpha 
mask. Use this shape to create a better bucket to hold the penguin.

- Make the world larger. You can make a larger landscape of your own. Try and copying and flipping the 
provided background. Be sure to make the ground wide enough. 
- Try making the catapult and arm smaller. You can scale these down in the scene. 
- If the whole scene is larger you will end up tossing the penguin in a higher arc above the top of the screen. 
In this case it is out of view. Remember that scaling the camera will zoom in or zoom out. In your `moveCamera()`
you can look at the y position of the `cameraTarget` and scale the camera to keep the penguin visible. 

# Summary

Congratulations on completing the bonus stage!
