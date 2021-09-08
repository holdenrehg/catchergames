---
title: "Learning Unity 2D dev as a web dev"
---

* TOC
{:toc}

I've complained about Unity before. Every time I have approached it, even after years of being a professional software developer, it just did not click for me. Working as a contractor and helping run a contracting software agency, I've had to jump into dozens of languages, frameworks, platforms, or tools without knowing much about them. Typically I'll fumble around with them for a few days or a week and get a rough idea about how to properly work with them. With Unity, the core ideas just never stuck and this never worked.

Well I have no idea what exactly happened, but within the last month I feel like it clicked.

I'm guessing the biggest shift for me was trying to ignore the GUI at first and figure out how the code works. That's where I feel most comfortable. I dug into the Unity docs, watched more YouTube videos than I'd like to admit, and now feel comfortable enough to be dangerous. Realistically I'm sure I understand less than 5% of the system since it's massive, but I've honed in on the core concepts as much as possible. A foundation to stand on is what I was looking for. This helped me escape tutorial-hell, googling every specific scenario. It brought me back to the days of freshman year of college.

For any non-games industry software devs or web devs feeling the same, I wanted to document some of the concepts I've worked through so far.

## This is not meant to be a tutorial

Heads up, this is not meant to be a step by step tutorial. It's really just a collection of tips, thoughts, and processes I've worked through along with some reference links to actual tutorials or documentation.

## The component system

When first learning, I kept seeing [references](https://docs.unity3d.com/520/Documentation/Manual/TheGameObject-ComponentRelationship.html) describe everything as a component. I didn't take this literally at first because "component" is such a generalized word in software. Seeing a class diagram really helped me out though. This was the first big ah-ha moment with Unity.

Literally, most things inherit from the "Component" unity core class.

![unity component class hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/unity-component-diagram.png)

So when building a simple game, you are building or reusing `Component`'s, organizing them into containers of `Component`'s (`GameObject`'s), and then organizing `GameObject`'s together into scenes. I guess a scene could be thought of as a container of containers of components? But let's not call it that please.

This changed my mindset. I started thinking in "components". For example, I started to build a top down, 2d prototype where a character with a sword fights spirits flying at him:

![top down prototype]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/top-down-prototype.png)

I started breaking down existing Unity components that I could use, components that I needed to make, how those components are going to live together, and how those components are going to interact with each other. My thought process basically went:

1. I need a character to appear on the screen. What component is best to render a sprite to the screen? I researched and looked at the [Unity Docs](https://docs.unity3d.com/Manual/Unity2D.html) to find the `SpriteRenderer` component. So I added a game object called Player which contains a sprite renderer component.
2. Then I needed the player to move. Again, what component will allow a 2d sprite to move? I looked at docs again and didn't see anything obvious, so I made a Movement component and added that to my Player. My Player game object now defers to two different components, one which tells it how to render a sprite and another for how to move that sprite around.

This is a simple example, but basically my process has stayed the same even as prototypes have gotten more complicated. I want to do XYZ. What component or components do I need to apply that functionality? What game objects are responsible for containing those components? What are the dependencies of the components? Do they need references to other components in the scene or on the same game object? etc.

**Component system links:**

- [Unity docs: Introduction to components](https://docs.unity3d.com/Manual/Components.html)
- [Unity Official Tutorial: Game Objects and Components](https://www.youtube.com/watch?v=9Nf2_ds5y8c)
- [Board to Bits Games: GameObjects & Components](https://www.youtube.com/watch?v=kH_piLCynto)
- [Creating and Using Scripts](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html)
- [How to get variables from other scripts in Unity](https://www.youtube.com/watch?v=7gRrujBjsFo)

**Component system takeaways:**

- `GameObject`'s are just containers of components.
- Scenes are containers of `GameObject`'s.
- `MonoBehvaior`'s are user defined components. Things you make to add functionality.
- "Scripts" are C# files where you define custom code. Usually `MonoBehavior`, `ScriptableObject`, or vanilla C# code.
- There are lots of reusable core components like `Tranform`, `SpriteRenderer`, `Camera`, `RigidBody`, etc.

---

## The GUI

After wrapping my head around the component system, game objects, and organizing scenes/hierarchies, then the coding side of things got easier. I could get back into the groove of pulling up VSCode and writing code like any other software project.

Then I had to deal with the GUI side of things. How does the code and the Unity GUI fit together?

There's so much packed into the GUI, but I've found myself staying withing a few sections of it most of the time:

**1. Scene Hierarchy**

![gui scene hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/scene-hierarchy.png)

The current scene is represented as a tree of game objects. This is where I started to understand game objects related to each other. Why would you nest one game object inside of another? So far, I've seen two reasons. 1. They have a dependency on each other. Say some component may search for a game object called "SpriteModel" inside of the game object its attached to. And 2. For organization. I guess this can be a bit dangerous for performance reasons in larger projects, but personally with small prototypes I've been creating empty game objects to stay organized. For example, I may add an empty game object called "Enemies" and nest all enemy game objects underneath that.

**2. Project Hierarchy**

![gui scene hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/project-hierarchy.png)

A list of project files as well as some Unity specific elements. You may see things show up here that would not show up when you open up a file explorer. Prefabs for example, which are a way to save and reuse game object across multiple scenes, or a way to dynamically instantiate game objects. As far as I'm aware, the data for these types of elements are defined in `.meta` files.

If you don't know where to get started with organizing these files (and don't want put everything in one big folder), there's plenty of methods out there. Reading code from other developers helps. In general, I try to break out common sense modules and keep everything related inside of it. A lot of people organize by type (all my behaviors in one folder, all my prefabs in one folder, all my animations in one folder, etc). It's best to do whatever works and feels natural for you.

**3. Scene**

![gui scene hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/scene.png)

A visual representation of your game objects. Each game object renders differently. For example, a game object with a camera component shows a camera icon. A game object with a sprite renderer component will show the sprite like in above.

Each component is positioned at a certain place in the world based on their `Transform` component which is required on every game object.

**4. Game**

![gui scene hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/game.png)

Then there's the game tab. It's the most straight forward section. When you run your game with the big Play button at the top, it runs the game in this window for you to play and test. At first, it's as simple as that.

**5. Inspector**

![gui scene hierarchy]({{ site.baseurl }}/assets/2021-09-07-unity-dev-for-web-devs/inspector.png)

Finally, the inspector. When clicking on a game object either in your scene or in your project the inspector will show you details about it. Remember that game objects are just collections of components, so it shows a dropdown section for each component attached to the gmae object.

Here is also where you can "glue" together any components you write with specific game objects. Wrote a `PlayerMovement` `MonoBehavior`? This is where you can add that `PlayerMovement` component to a game object.

In the example above it has 4 components:

- Transform
- Sprite Renderer
- Animator
- Box Collider 2D

There's a lot more that can be done with the inspector, but at its core it's a way to inspect game objects, change variables associated with those components, add new components, delete existing components, debug, etc.

**GUI links:**
- [Unity docs: Learning the interface](https://docs.unity3d.com/Manual/UsingTheEditor.html)
- [Unity docs: The inspector window](https://docs.unity3d.com/Manual/UsingTheInspector.html)
- [Unity docs: The scene view](https://docs.unity3d.com/Manual/UsingTheSceneView.html)

---

## Lifecycles

Since Unity games are just large collections of components for the most part, each component has the ability to hook into events that fire at certain points in the lifecycle of the game. Every game runs in a loop. It keeps cycling through the same functions over and over, adjusting to changes in state triggered outside factors. This could be a user providing some input, time moving forward, etc.

Unity, with it being a game engine full of helpful utilities, also automatically does a bunch of things within its game loop. One of those being providing hooks for you as a developer, since you aren't directly writing the game loop yourself.

When a Unity game starts up, it goes through the following steps:

- Startup: the initialization of the system and components.
- Updates: beginning of the game loop, with each iteration through the cycle being 1 frame.
    - Physics
    - Input event
    - Game logic
    - Scene rendering
    - Gizmos
    - GUI
    - End of frame
    - Pausing
- Teardown: when application quit is triggered, enters Teardown.

The most common hooks I've run into so far are:

- `Awake`: called once per application startup, before any `Start` function.
- `Start`: called once before the first frame update.
- `FixedUpdate`: used for the physics engine and is independent from the frame rate. Can be called multiple times per frame.
- `Update`: called once per frame.
- `LateUpdate`: called once per frame, after `Update` is done. Can be helpful is some calculation in Update needs to complete first.
- `OnDestroy`: Called on the last frame before this object is destroyed.
- `OnApplicationQuit`: Called on all components before the game shuts down.

**Lifecycles links:**
- [Unity docs: execution order](https://docs.unity3d.com/Manual/ExecutionOrder.html)
- [Infalliable Code: A Quick Look at Unity's Application Lifecycle](https://www.youtube.com/watch?v=jOP0IfarD68)

---

## Controller input

When working in web dev, there's a good amount of IO happening at any given moment. A web application may be taking in user input via the browser, but many applications are also dealing with server side IO. The application may be polling external services, constantly communicating with a data layer, using webhooks to handle events, running and communicating via websockets, etc.

With non networked, non multiplayer, simpler games, it seems that developers primarily need to worry controller input from the user though. This is one piece that feels much simpler than the web applications I've dealt with. Unity provides a couple of features for dealing with this type of input:

1. The `Input` class which is a facade for the `InputManager.
2. The "Input System" package introduces a couple of years ago for handling more complex input setups.

I've only focused on the input manager at this point and is fairly common sense. `if Input.GetKey(...)` then do something.

**Controller input links:**
- [Input Manager](https://docs.unity3d.com/2019.4/Documentation/Manual/class-InputManager.html)
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/index.html)
- [Jason Weimann: How to use the New Unity Input System](https://www.youtube.com/watch?v=zIhtPSX8hqA)

---

## Wrapping up

I've been working through quite a few concepts in game development in general, as well as Unity API specific features. There's plenty more to talk about, but there's more I want to learn before typing up more thoughts.

If you're interested, here's some of the other core concepts I've been working through:

- Player movement mechanics, simple 2d physics.
- 2D camera and lighting techniques (using the URP).
- Persistent data (save/load data management).
- User interfaces.
- Unity specific editor tools.
- Dialog systems and translations.

I've been able to learn quite a bit through these resources. Check them out:

- [Unity Manual](https://docs.unity3d.com/Manual/Unity2D.html)
- [C# Language Reference](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/)
- [Jason Weimann](https://www.youtube.com/c/Unity3dCollege)
- [Brackeys](https://www.youtube.com/c/Brackeys): defunct, but still plenty of relevant info on their YouTube.
- [Blackthornprod](https://www.youtube.com/c/Blackthornprod)
- [Jimmy Vegas](https://www.youtube.com/c/JimmyVegasUnity/videos)
- Open Source Code to read through:
    - [Diablerie](https://github.com/mofr/Diablerie)
    - [Newbark](https://github.com/itsjavi/newbark-unity)
    - [Unity Technologies: Chop Chop](https://github.com/UnityTechnologies/open-project-1)
