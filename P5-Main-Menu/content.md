---
title: Adding the main menu
slug: main-menu
---

Now you are going to setup a main menu screen. This start menu will then lead 
the player into the game play scene that you will develop in the next chapter.

The scene will be made of two parts:

1. *MainMenu.swift* - contains all of the code that controls the scene.
2. *MainMenu.sks* - is a visual editor that that holds all of the objects in the scene.

# Adding the Main Scene

You are going to create a new *Scene* for the main menu.

> [action]
> Create a new SKS file (`File > New > File > SpriteKit Scene`) and name it `MainScene.sks`
> Set *Scene Size* to `(568,320)`
>
> ![MainScene attributes](../Tutorial-Images/p5-01-menu-scene.png)
>

## Adding the background

> [action]
> Drag *menubackground.png* to the scene and snap it to the bottom-left corner.
> Set *Z-Position* to `-1` to ensure the background is always at the back.
>
> ![menu background](../Tutorial-Images/p5-02-menu-scene.png)

## Add MSButtonNode

Add the MSButtonNode class to your project. If you've improved the *MSButtonNode* class 
in the *Hoppy Bunny Tutorial* and made it epic. Please feel free to reuse your copy in 
this project.

> [action]
> [Download MSButtonNode.swift](../MSButtonNode.swift) and drag the file into your project, 
> ensuring *Copy items if needed* is checked.
>

# Creating the play button

The main menu needs a play button to trigger the transition into the *GameScene*.

> [action]
> SpriteKit does not provide any button objects, so you will need to use our assets as you 
> did in the *Hoppy Bunny Tutorial*.
> Drag *button.png* into the scene, place it wherever looks good to you.
> Then set the name of the button to `buttonPlay`. 
>
> ![Setting a custom class](../Tutorial-Images/p5-03-button-name.png)
>
> Now create a code connection to link this button to your code. Set the 
> the *Custom Class* to `MSButtonNode`:
>
> ![Setting a custom class](../Tutorial-Images/p5-03-msbuttonnode.png)
>

## Coding the main scene

Let's add a new *MainMenu.Swift* file to facilitate functionality for 
*MainMenu.sks*.

> [action]
> Create a new empty Swift file (`File > New > File > Swift File`) and name it 
> `MainMenu.swift`
>
> ![Add MainMenu.swift](../Tutorial-Images/p5-05-mainmenu-swift.png)
>
> Ensure your new class reads as follows. Add the following to *MainScene.swift*:
>
```
import SpriteKit
>
class MainMenu: SKScene {
>
/* UI Connections */
var buttonPlay: MSButtonNode!
>    
    override func didMove(to view: SKView) {
        /* Setup your scene here */
>
        /* Set UI connections */
        buttonPlay = self.childNode(withName: "buttonPlay") as! MSButtonNode
>      
    }
}
```
>

This defines the class for this scene. It also defines a var to hold a reference to the button node. When the scene 
loads we look for the node with the name "buttonPlay" and assign it to the var. 

# Launching the Main Scene

By default a new game project will load the *GameScene*, you need to change this to 
load *MainMenu* instead. The entry point for your game is `GameViewController.swift`. 

> [action]
> Open *GameViewController.swift* and modify the following line inside `viewDidLoad()`:
>
```
if let scene = SKScene(fileNamed: "GameScene") {
```
> Replace with:
```
if let scene = MainMenu(fileNamed: "MainMenu") {
```

The `viewDidLoad()` method in `GameViewController` is called when this view controller 
appears on the screen, at which time we load the first scene: `MainMenu`. 

Here you created an instance of *MainMenu.swift* which is an `SKScene`. When loading a Scene 
you can also load it with a sks file using `SKScene(fileNamed:)`. In this case you used: 
`MainMenu(fileNamed: "MainMenu")` to create an instance of MainMenu (from MainMenu.swift)
with the `MainMenu.sks` scene. 

Run your game...

![Screenshot main menu](../Tutorial-Images/p5-04-button-test.png)

If the Simulator shows your game in portrait choose Hardware > Rotate Right 
(or Command+Right-arrow)

Looks good, you can even touch the button. However, it doesn't do anything other than 
print: "No button action set".

## Load the Game Scene

> [action]
> Add an action to handle when button is tapped touched. Add the 
> following inside `didMove(to view:)` below `buttonPlay = ...`:
> 
```
/* Setup restart button selection handler */
buttonPlay.selectedHandler = {
>    
    /* 1) Grab reference to our SpriteKit view */
    guard let skView = self.view as SKView! else {
        print("Could not get Skview")
        return
    }
>    
    /* 2) Load Game scene */
    guard let scene = GameScene(fileNamed:"GameScene") else {
        print("Could not make GameScene, check the name is spelled correctly")
        return
    }
>    
    /* 3) Ensure correct aspect mode */
    scene.scaleMode = .aspectFill
>   
    /* Show debug */
    skView.showsPhysics = true
    skView.showsDrawCount = true
    skView.showsFPS = true
>    
    /* 4) Start game scene */
    skView.presentScene(scene)
}
```
>
> The button's `selectHandler` is a code block that runs when the button tapped. 
> The code in this block gets a reference to the view **(1)**, we use guard here since `view`
> is an optional. Next load the `GameScene` class with *GameScene.sks* **(2)**. This step also returns 
> an optional so again we use `guard`. If all of that worked you set some options on the new scene
> **(3)**. Last show the scene by calling `presentScene()` **(4)**.
> 

Run the project...

You should now be able to touch the **Play** button and be presented with an empty 
*GameScene*.

# Summary

You've learned to:

- Implement multiple scenes
- Change the default launch scene
- Create a custom button and launch another scene

In the next chapter you will building the game scene.
