SceneSetup
==========

This Unity3D-Project tries to bypass Unity3Ds loading behaviour.

As of Unity3D 2020.3.19f1 loading happens this way:
  - Load Scene (Progress _0.0f to 0.9f_)
  - Scene Activation (_0.9f to 1.0f_)
  - During Scene Activation **Awake, OnEnable & Start** are **called on all GameObjects** within **one frame** (see [Unity3D Execution Order](https://docs.unity3d.com/Manual/ExecutionOrder.html))
  - This **causes stutter** if the loaded Scene is bigger than a few calls (Animation freezes etc.)

I tried to bypass this behaviour this way:
  - Load Scene (Progress _0.0f to 0.9f_)
  - Scene Activation (_0.9f to 1.0f_)
  - During Scene Activation **Awake, OnEnable & Start** are **called only on SceneSetupManager** within **one frame**
  - During this one frame the **SceneSetupManager searches for all** GameObjects with the Script **MonoEntity** and subscribes to its event **MonoEntityLoaded**
  - The **SceneSetupManager checks in its Update-Method** for all MonoEntities to finish (basically a **Start over multiple frames**)
  - This **causes minimal stutter** if Awake & OnEnable are only used for negligible operations (e.g. subscribe to an event)

I also added some _fluff_ (Animations, Press key to continue, fake MonoEntities, added loading time, etc.) to better bring my point across.

You can see this behaviour in action in the scenes **Scene 01** & **Scene 02**.\
Think about them as a Main-Menu (01) and a Game-Scene (02). Main-Menu loads fast but loading the big Game-Scene takes longer.

**You don't want your players to wait without feedback** or **the game to crash if the player is impatiently clicking around**.\
That's when you can use this project as a reference or just copy it as is.

If you find any errors or have suggestions for improvement please let me know.
