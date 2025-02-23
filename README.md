## The Frost Of Endless Tomorrow

You wake up in a nightmare.  
Play as **Elliot**, a man stuck in a loop of frozen hellscapes, hallucinations, and fractured memories. One moment you’re in a rotting house, the next a forest where the snow bleeds. Every corner hides clues to why you’re here—and why you can’t leave.

Time loops. Fading memories. A world that _wants_ you to forget.  
How far will you go to escape a tomorrow that never comes?
Will you break the cycle, or will you succumb to the frost?

### Inspiration
The story of this game was inspired with themes of an psychological-thriller novel, that i was currently working on. 
## 
This project was built with a focus on **modular systems** and **dynamic event management** to create an immersive and replayable experience. With easy to **expand systems**.

Below, you’ll find an overview of the core systems and mechanics that bring this haunting world to life.
# Core Systems Overview
### Interaction System
`IInteract` - interacting with door, keys, lever, buttons and other interactable objects. 
`ILightSource` - Used to run "`FlickerLight()`" (flickering effect) and "`DestroyBulb()`" (On encounter with ghost the first time, triggered.)
`IPlayer` - Used to trigger functions inside `PlayerCharacter` script like `Hallucination()` function, which enables `PostProcess()` component. 

## Event Manager System
`EventManagment` GameObject+Script is used to trigger/manage events

**EventsEnumeration**
- LightFlicker
- Halucinations
- BlackOut
- SoundPlay
- ShowMessage
- JumpScareEvent
- DisablePlayerTorch 

Each Instance of `EventManagerSystem` of has, variables that can be edited based on instance in editor.
- `EventIndex` (Events Enumeration datatype) - this one decides which main thing the particular EventObject would trigger before getting destroyed.
- `SendMessageToo?` (bool)
- `PlaySoundAsWell?` (bool)
- ` DisablePlayerLights?` (bool) and `DisableLightsFor` (float)
- `HallucinationsAsWell?` (bool)

based on which bool is active, aside from the Event in `EventIndex` in getting triggered you can send/show/trigger multiple functions based on use-case. (like at one place i might desire to - show an message+sound), in one i might want (sound+hallucination+lightflicker) - so according to the scene dynamic need we can

`OnBeginOverlap()` when condition `ActorHasTag("Player")` is met, triggers the (Event in `EventIndex`) & Other Events based on which bool active  
## User Interface System
**Functions List**
- `UILoadChapter(Int LevelIndex, Int Duration)` - This function is responsible for showing opening text of every new chapter
- `UIMessageDisplay(Text Text)` - responsible for showing dialogues and informative texts (ex-picked up Kitch key, You need ABC to open this door.)
- `UIDisplayDiary(Text Title, Text Entry)` - Trigged when player interacts with an DiaryActor/NotesActor/etc - to display it's content.
- `UIUnDisplayDiary()`
- `UINoobsGuide()` - Displays UI for which key is responsible for what action (RMB-Interact, X-DropItem, LMB - UVFlashlight)

# PlayerCharacter Script & GameObject
**Some Important Functionalities**
- Responsible for taking keyboard inputs and running functionality relating to said function
- There's an `SphereCollider` attached that 
	- `OnBeginOverlap()` - checks for any interactable object (**`Does Object Implement Interface? == IInteract`**), to give player heads-up w/ UINoobsGuide() for how to interact with the object.
	- When user press `InputActionKey` for `Interact`, we iterate through all overlapping objects to check for (**`Does Object Implement Interface? == IInteract`**) if yes, then Trigger `Interact(this)` function utilizing the `IInteract` interface
- Also there's an `postprocess` component attached to this, that is enabled every time we wish to display hallucinations and other effects.
- Two Spot Light Components 
	- one with shorter cone angle (serves as flashlight) 
	- other with bigger cone angle (at low illuminosity) (to give an general ambient illumination to surrounding) for an much better experience
### Keys/Possessable PickUp System
The data of whether player possess an key/possessable item is stored in an Bool Array, whose indexes correspond to which is stored in an Enum (like 0=BedRoomKey 1=KitchenKey,..)

When `IInteract()` function is run, it updates `BoolArray` set value at `(int)EnumGameObject` 

