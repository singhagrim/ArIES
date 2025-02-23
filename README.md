# The Frost Of Endless Tommorow

`IInteract` - interacting with door, keys, lever, buttons and other interactable objects. 
`ILightSource` - Used to run "`FlickerLight()`" (flickering effect) and "`DestroyBulb()`" (On encounter with ghost the first time, triggered.)
`IPlayer` - Used to trigger functions inside `PlayerCharacter` script like `Hallucination()` function, which enables `PostProcess()` component. 
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
The data of whether player possess an key/possesable item is stored in an Bool Array, whose indexes correspond to which is stored in an Enum (like 0=BedRoomKey 1=KitchenKey,..)

## Event Manager System
