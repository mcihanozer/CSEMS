# C# Event Management System (CSEMS)

C# event management system with in-house events and event managers. It can be used for any C# projects, but originally aimed for Unity.

CSEMS is currently highly unstable and still in evolution process.

This document provides information about CSEMS' core classes and also provides examples based on a hexfall game -user-defined event managers example (aka [Hexic by  Alexey Pajitnov](https://en.wikipedia.org/wiki/Hexic)).

First, we will talk about the core base classes, then give examples about the event managers, continue with in-house events, and finish by showing how you can add your own events and event managers.

## Table of Content
### 1. [Information About Base Classes](https://github.com/mcihanozer/CSEMS#information-about-base-classes)
####      1.1. [Core classes](https://github.com/mcihanozer/CSEMS#core-classes)
#### [1.2. Observer design pattern based](https://github.com/mcihanozer/CSEMS#12-observer-design-pattern-based-1)
#### [1.3. Base classes for event managers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#base-classes-for-event-managers)
### [2. Event Managers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#event-managers)
#### [2.1. System related event managers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#system-related-event-managers)
#### [2.2. Example user-defined event managers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#22-example-user-defined-event-managers)
### [3. Events](https://github.com/mcihanozer/CSEMS/blob/master/README.md#3-events)
#### [3.1. System event types](https://github.com/mcihanozer/CSEMS/blob/master/README.md#31-system-event-types)
#### [3.2. Example user-defined events](https://github.com/mcihanozer/CSEMS/blob/master/README.md#32-example-user-defined-events)
### [4. Event Handlers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#4-event-handlers)
### [5. An Example: HexfallGameController](https://github.com/mcihanozer/CSEMS/blob/master/README.md#5-an-example-hexfallgamecontroller)
#### [5.1. Step 1: Creating the events](https://github.com/mcihanozer/CSEMS/blob/master/README.md#51-step-1-creating-the-events)
#### [5.2. Step 2: Creating the event handlers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#52-step-2-creating-the-event-handlers)
#### [5.3. Step 3: Creating the event managers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#53-step-3-creating-the-event-managers)
#### [5.4. Step 4: Implementing event handlers](https://github.com/mcihanozer/CSEMS/blob/master/README.md#54-step-4-implementing-event-handlers)

## 1. Information About Base Classes

Here, you will find some informarion about the core classes you should be aware of for constructing your own code.

### 1.1. Core classes

Each class should be either inherited from **LB_BaseGameObject**, which is inherited from **LB_BaseObject**, or at least implement **LB_IBaseGameObjectFunctionality** interface. Otherwise, they cannot receive essential calls, such as **_LB_Awake()_**, **_LB_Start()_**, **_LB_Update()_**, and so on.

If you are familiar with Unity, these methods have the same purpose with **_MonoBehaviour.Awake()_**, **_MonoBehaviour.Start()_**, and **_MonoBehaviour.Update()_**.

#### 1.1.1. LB_BaseObject

Base class for any objects in the project. This class includes some common members and properties, such as id, name, etc.

#### 1.1.2. LB_IBaseGameObjectFunctionality

Interface that provides the esential methods for a game object class.

#### 1.1.3. LB_BaseGameObject

Base class for the classes that will take a part in the game related tasks, but will not need to be inherited from MonoBehaviour of Unity, such as game managers, event managers, and so on.

LB_BaseGmeObject provides the common blue print methods, needed members, and common functionality.

### 1.2. Observer design pattern based

CSEMS is designed based on observer design pattern. Event managers are initiated with **Subject** class and event handlers are initiated with **Observer** class.

#### 1.2.1. Subject and Observer classes

Generic base Subject and Observer classes for an event-driven system.

Subject object maintains a list of its dependents, called observers. The observers register with the subject, so they can get notified when a predefined condition, event, or state change occurs. The subject automatically notifies all observers by calling one of their methods (callbacks).

Any number of observers can observe a subject. So, this is one-to-many relationship.

### Base classes for event managers

All event managers (including user-defined ones) are inherited from generic **_BaseEventManager_** and automatically registers themselves to **_MainEventManager_** to receive frame calls (_Update()_, _callOnFrame()_, and so on). **_MainEventManager_** is a singleton class and it manages all event managers in the project. Sub-event managers operates via **_MainEventManager_**.

## 2. Event Managers

Here, we will talk about example event managers provided. They all define their own related events and inherit themselves from **_BaseEventManager_**. Event-handling classes (the classes that want to listen a particular event) implement **_Observer_** interface and register themselves to related event manager.

### 2.1. System related event managers

**ButtonEventManager:** is a singleton event manager for handling **_LB_Button_** events. **_LB_Button_** events are in-house events for handling Unity buttons better and easier. We will cover **_LB_Button_** events later detailed.

**SystemEventManager:** is a singleton system event manager for handling system related events, such as quitting.

**MouseEventManager:** is a singleton event manager for handling mouse events. This manager works via Unity's Input class. It also provides an in-house mouse hold-and-move event.

**TouchEventManager:** is a singleton event manager for handling touch events. This manager works via Unity's Input class.

### 2.2. Example user-defined event managers

**HexfallUIEventManager:** is a singleton event manager for managing Hexfall game UI events.

**HexfallEventManager:** is a singleton event manager for Hexfall related event management.

## 3. Events

All events are inherited from **_LB_Event_** class and define their own event type and define other specialities if needed.

### 3.1 System event types

Now, we will talk about system and user-defined events.

#### 3.1.1. LB_Event

_LB_Event_ is the base event class both for system and user-defined events. It defines parent event information (system, touch, mouse, user-defined, and so on), event type (mouse down, touch up, etc.), and an **_object_** type member in case you need to hold extra information.

#### 3.1.2. LB_ButtonEvent

_LB_ButtonEvent_ is an in-house button event for handling click, button down, and button up events. **_LB_Button_** script is added to the buttons in the project and related callbacks (click, button down, and button up) are assigned via Unity editor. These callbacks automatically creates related events and register them to **_ButtonEventManager_**.

#### 3.1.3. LB_SystemEvent

_LB_SystemEvent_ is used for handling system related actions, such as quit request, processing quit request and so on.

#### 3.1.4. LB_MouseEvent

_LB_MouseEvent_ is an in-house mouse event used for handling mouse down, mouse up, and mouse hold and move events. It also provides information about mouse x and y positions in pixel coordinates.

#### 3.1.5. LB_TouchEvent

_LB_TouchEvent_ is an in-house touch event used for handling touch down, touch up, and touch and move events. It also provides information about touch x and y positions in pixel coordinates.

### 3.2. Example user-defined events

#### 3.2.1. HexfallUIEvent

_HexfallUIEvent_ provides events such as main menu, go game preparation, go in game, and so on. These events are used to change between game menus.

#### 3.2.2. HexfallEvent

_HexfallEvent_ provides events such as start game, quit game, score, generate bomb, generate hex, and so on. These events are used to handle actions in the game, mostly by various game controllers.

## 4. Event Handlers

Event handlers are interfaces based on generic **_LB_Observer_** class mentioned in **_Subject and Observer classes_** section. Each event handler inherits **_LB_Observer_** by its event type. Each class that aims to handle one or more events implements **_HandleEvent(EventType e)_** method provides by the related event handler.

For example, **_LB_IButtonHandler_** is inherited from **_LB_Observer_** interface with **_LB_ButtonEvent_** event. A class wanting to handle **_LB_Button_** events needs to use **_LB_IButtonHandler_** interface and to implement **_HandleEvent(LB_ButtonEvent e)_** method coming from  **_LB_IButtonHandler_** interface.

You can find more examples in the list below:

- **LB_IButtonEventHandler:** inherites `LB_Observer` with `LB_ButtonEvent`, provides `void HandleEvent(LB_ButtonEvent e)` method
- **LB_ISystemEventHandler:** inherites `LB_Observer` with `LB_SystemEvent`, provides `void HandleEvent(LB_SystemEvent e)` method
- **LB_IMouseEventHandler:** inherites `LB_Observer` with `LB_MouseEvent`, provides `void HandleEvent(LB_MouseEvent e)` method
- **LB_ITouchEventHandler:** inherites `LB_Observer` with `LB_TouchEvent`, provides `void HandleEvent(LB_TouchEvent e)` method
- **IHexfallUIEventHandler:** inherites `LB_Observer` with `HexfallUIEvent`, provides `void HandleEvent(HexfallUIEvent e)`method
- **IHexfallEventHandler:** inherites `LB_Observer` with `HexfallEvent`, provides `void HandleEvent(HexfallEvent e)` method

## 5. An Example: HexfallGameController

Let us assume we need a game controller that needs to handle mouse and touch events. Here, we will show how to create these events, event managers, and related event handlers.

For constructing such system

1. We need to define the related events based on **_LB_Event_**,
2. Its event handler based on **_LB_Observer_**,
3. Its event manager based on **_LB_Subject_**,
4. and implement the related **_HandleEvent(EventType e)_** methods.


### 5.1. Step 1: Creating the events

Here, we will define mouse and touch events, their managers, and event handlers. We will end the example by implementing mouse and input event handler methods.

#### 5.1.1. Creating mouse and touch events

Let's assume we want to design a mouse event providing mouse down, mouse up, and mouse hold and move events along mouse x and y positions in pixel coordinates. We can achieve this goal by the following steps:

- [x] Inherit from LB_Event
- [x] Define event types and event type field
- [x] Define other specifications needed
- [x] Define constructors

These are the general steps we need to follow whenever we need to introduce a new user-defined event.

When we follow the general steps, we get a mouse event class as below:

```
// Step 1: Inherit from LB_Event
public class LB_MouseEvent : LB_Event
{
  // Step 2: Define event types
	public enum MOUSE_EVENTS : uint
	{
		 MOUSE_DOWN, MOUSE_UP, MOUSE_HOLD_MOVE
	}

  // Step 2: Define event type field
	public MOUSE_EVENTS EventType
  {
		  get { return (MOUSE_EVENTS)EVENT_TYPE; }
       set { EVENT_TYPE = (uint)value; }
   }

   // Step 3: Define other specifications
   public float MOUSE_POS_X;
   public float MOUSE_POS_Y;

  // Step 4: Define constructors
	public LB_MouseEvent() : base(LB_EVENTS.MOUSE_EVENT)
  {
		MOUSE_POS_X = -1;
		MOUSE_POS_Y = -1;
  }

	public LB_MouseEvent(
		                    MOUSE_EVENTS eventType,
		                    float mousePosX, float mousePosY,
		                    string extraInfo = ""
	                    )
		: base(LB_EVENTS.MOUSE_EVENT, (uint)eventType, extraInfo)
  {
		MOUSE_POS_X = mousePosX;
		MOUSE_POS_Y = mousePosY;
  }
}
```
We would define touch event as in the same way. You can refer to other event examples provided if you need more detail.

### 5.2. Step 2: Creating the event handlers

Since, we now have our events, we can define our event handlers. Event handlers are interfaces based on generic **LB_Observer** interface. Each event handler defines itself by an associated event and provides an event handler method via **LB_Observer** interface.

#### 5.2.1. Creating mouse and touch event handlers

Let's consider mouse and touch event handlers based on what we've covered so far:

```
// Interface for mouse events
public interface LB_IMouseEventHandler : LB_Observer<LB_MouseEvent>
{ }

// Interface for touch events
public interface LB_ITouchEventHandler : LB_Observer<LB_TouchEvent>
{ }
```
C'est tout! Defining event handlers in CSEMS is very straightforward.

### 5.3. Step 3: Creating the event managers

Event managers are basically the subjects in observer pattern. CSEMS encapsulates the subject class via **BaseEventManager**. You can still create subject classes based on **LB_Subject** if you need for different purposes, such as you need a messaging system, but for creating an event manager that will work in CSEMS, you need to inherit your user-defined event manager from **BaseEventManager**.

Generic **BaseEventManager** manager requires two generic types; an _EventType_ based on _LB_Event_ and an _EventHandlerType_ based on _LB_Observer<EventType>_. Thus, when we create a mouse event manager `MouseEventManager`, we will inherit it from `BaseEventManager` as `public class MouseEventManager : BaseEventManager<LB_MouseEvent, LB_IMouseEventHadler>`.
  
Let's consider an easier example this time since **MouseEventManager** is a bit complicated. Let's assume we have our `HexfallEvent` and `IHexfallEventHandler`, and we want to write a `HexfallEventManager`. We could write this class as below:

```
// Singleton event manager for Hexfall related event management
public class HexfallEventManager : BaseEventManager<HexfallEvent, IHexfallEventHandler>
{
	// Thread-safe without using locks, not quite lazy
	private static readonly HexfallEventManager instance = new HexfallEventManager();
   
	static HexfallEventManager() { }

	private HexfallEventManager() { }

	public static HexfallEventManager Instance
    {
        get
        {
            return instance;
        }
    }  
}
```

The singleton is a design choice and you do not need to define your event manager as singleton. Also, if you do not need some special requirements, as **MouseEventManager**, you also do not need to override any methods. **BaseEventManager** provides you all functionality you need for an event-driven system, such as adding event, notifying listerners, registering to **MainEventManager**, and so on.

### 5.4. Step 4: Implementing event handlers

Let's go back to our mouse and input events and their event handlers along their event managers. In this part, we will focus on implementing a game controller that receives mouse and touch events.

There are two steps to achieve this functionality:
 1. Implementing related event handlers
 2. Registering to the related event managers
 
 for having the chance of implementing the related event handlers, first we need to inherit from the related event handlers. In our case, these are `LB_IMouseEventHandler` and `LB_ITouchEventHandler`. So, we will define our `HexfallGameController` class as `public class HexfallGameController : LB_BaseGameObject, LB_IMouseEventHandler, LB_ITouchEventHandler`.
 
 After having the event handlers, implementing them is straightforward and not any different than implementing any interface method. Let's assume, we have the following actions depending on the events we receiver:
 
 - **_MouseDown_** or **_TouchDown_**: Take input
 - **_MouseUp_** or **_TouchUp_**: Handle hexagon selection
 - **_MouseHoldAndMove_** or **_TouchMove_**: Rotate selected hexagon
 
 so, we would implement the event handlers as below:
 
 ```
public void HandleEvent(LB_MouseEvent e)
	{
		if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_DOWN)
		{
			game.TakeInput();
		}
		else if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_UP)
		{
			game.HandleHexagonSelection();
		}
		else if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_HOLD_MOVE)
		{
			game.HandleHexagonRotation();
		}
	}

	public void HandleEvent(LB_TouchEvent e)
	{
			if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_DOWN)
		{
			game.TakeInput();
		}
		else if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_UP)
		{
			game.HandleHexagonSelection();
		}
		else if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_MOVE)
		{
			game.HandleHexagonRotation();
		}
	}
```

Now, we are at the final step to integrate CSEMS fully. It is time to register our class, `HexfallGameController`, to the related event managers. This step is inevitable and if you do not register your class to the related event managers, you cannot receive any event calls. It is like expecting phone calls without having a phone number.

It is better to handle this registeration after being sure the core system is ready. Since our event managers are singleton, we can handle this in **LB_Awake()** method having the same order and logic as Unity's **Monobehaviour.Awake()**. If the current event managers were not singletons than it would be better to handle the registeration process in **LB_Start()** the equivalent of **Monobehaviour.Star()**.

Here is the full code of `HexfallGameController`:

```

public class HexfallGameController : LB_BaseGameObject, LB_IMouseEventHandler, LB_ITouchEventHandler
{
	HexfallGame game;

	public override void LB_Awake()
	{
#if (UNITY_EDITOR || UNITY_STANDALONE)
		MouseEventManager.Instance.Attach(this.SystemId, this);
#elif (UNITY_IOS || UNITY_ANDROID)
		TouchEventManager.Instance.Attach(this.SystemId, this);
#endif

		game = new HexfallGame();
		game.LB_Awake();

	}

	public override void LB_Start()
	{
		game.LB_Start();
	}

	public override void LB_OnDestroy()
	{
		game.LB_OnDestroy();
	}

	public void HandleEvent(LB_MouseEvent e)
	{
		if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_DOWN)
		{
			game.TakeInput();
		}
		else if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_UP)
		{
			game.HandleHexagonSelection();
		}
		else if (e.EventType == LB_MouseEvent.MOUSE_EVENTS.MOUSE_HOLD_MOVE)
		{
			game.HandleHexagonRotation();
		}
	}

	public void HandleEvent(LB_TouchEvent e)
	{
			if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_DOWN)
		{
			game.TakeInput();
		}
		else if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_UP)
		{
			game.HandleHexagonSelection();
		}
		else if (e.EventType == LB_TouchEvent.LB_TOUCH_EVENTS.TOUCH_MOVE)
		{
			game.HandleHexagonRotation();
		}
	}
}
```
