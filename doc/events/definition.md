# Events defined by Turtle Universe
## Common Fields
* Category - Category of the event.
* Source - Source of the event.
* Name - Name of the event.

### Parameters (Fields)
* Command - The expected command to be executed. 

## General Events (Category: 0)
### Source: (null)
#### Type: Restart
* When the user restarts the simulation. It will be executed before "setup" - even without a "setup", it will be triggered.

#### Type: Load
* When the user loads the simulation from a world state. In such a case, setup will not be triggered, but this event will.

#### Type: Play/Pause
* When the user plays, resumes, or pauses the simulation manually. Will not trigger when the model is opened.

#### Type: BeforeSave/Save
* When the user saves the simulation, or right before she/he is going to do so. 
* In the event of auto-saving, there will be a field called "AutoSave". 
* Example:
```json
{ 
	Category: 0, 
	Source: null,
	Type: "Save", 
	Fields: {
		AutoSave: true
	}
}
```

### Type: Tutorial
* When the user clicks the tutorial icon and there is no minimized dialog available.

### Type: Frame
* Whenever the model finishes another tick frame.
	* A tick frame may contain multiple ticks and is determined by the rendering layer. 
	* We strongly recommend all tutorial designers use this event to accelerate the running.
* Parameter Ticks: How many ticks have we executed in this single frame?

### Type: Tick
* Whenever the model finishes another tick.

### Type: Focus
* Whenever the user focuses an agent.
* Parameter Type: Type of the agent. ("Patch", "Link", "Turtle")
* Parameter Agent: Instance of the agent. 
* Parameter Action: Action of the user. ("Inspect", "Watch", "Follow", "Ride")

### Type: Unfocus
* Whenever the user stops to focus an agent.
* Parameter Type: Type of the agent. ("Patch", "Link", "Turtle")
* Parameter Agent: Instance of the agent. 
* Parameter Action: Previous action of the user. ("Inspect", "Watch", "Follow", "Ride")

### Source: Speed
#### Type: Change
* When the user manually changes the speed. Only effective changes will be sent - that means one event at most for each execution round.
* Parameter Value: Designated speed in times (1x = 10 ticks/sec). In the real world, the device might not be able to catch up with that speed. 
	* Note that the execution speed is always not accurate; the number displayed is "how fast we executed in recent frames averagely".
* Example:
```json
{ 
	Category: 0, 
	Source: "Speed",
	Type: "Change", 
	Fields: {
		Value: 10
	}
}
```

## Layout Events (Category: 1)
* For all layout events, the field "Source" is always the targeted component's name.

### Components
* Plots: Full plot region.
* Widgets: Widgets card.
* Code: Code card.
* NetTango: NetTango card.
* More: More card.
* Output: Output card.

#### Only available when hiding by code
* Cards: All cards. 
* Everything: Cards and plots.

### Type: Show
* Triggers when a component is shown by the user's interaction.
* Example:
```json
{ 
	Category: 1, 
	Source: "Plots",
	Type: "Show"
}
```

### Type: Hide
* Triggers when a component is hidden by the user's interaction.
* Example:
```json
{ 
	Category: 1, 
	Source: "More",
	Type: "Hide"
}
```

## Widget Events (Category: 2)
* Since NetLogo is case-insensitive in terms of variable names, **source** in events will always be lower-cased.

### Type: Click
* Supported by Button widgets. It will always trigger, even if it is not legitimate. 
	* For example, the event will trigger if the button requires tick initialization and the tick is not initialized. However, the command field will be null.
* Example:
```json
{ 
	Category: 2, 
	Source: "some-button-name", 
	Type: "Click", 
	Fields: {
		Command: "print \"hello world\""
	}
}
```

### Type: Change
* Supported by all widgets with a value. Only effective changes will be sent - that means one event at most for each execution round.
* Parameter Value: Designated new value for the widget with variable {Name}.
* Example:
```json
{ 
	Category: 2, 
	Source: "some-global-variable", 
	Type: "Change", 
	Fields: {
		Value: 12345
	}
}
```

## Plot Events (Category: 3)
* For all plot events, the field "Source" is always the targeted plot's name.

### Type: Activate
* Triggers when a plot is activated by the user's interaction.
* Example:
```json
{ 
	Category: 3, 
	Source: "some-plot", 
	Type: "Activate"
}
```

## Dialog Events (Category: 4)
* For all dialog events, the field "Source" is always the targeted dialog's name.

### Type: Show
* Triggers when a dialog is displayed, whether explicitly by user interaction or implicitly. 
* Example:
```json
{ 
	Category: 4, 
	Source: "some-dialog", 
	Type: "Show",
	IsManual: true
}
```

### Type: Minimize
* Triggers when a dialog is minimized, whether explicitly by user interaction or implicitly. 
* Parameter IsManual: Whether this is triggered by user's direct interaction.
* Example:
```json
{ 
	Category: 4, 
	Source: "some-dialog", 
	Type: "Minimize",
	IsManual: true
}
```

### Type: Hide
* Triggers when a dialog is hidden, whether explicitly by user interaction or implicitly. 
* Parameter IsManual: Whether this is triggered by user's direct interaction.
* Parameter IsMinimized: Whether the dialog is already minimized.
* Example:
```json
{ 
	Category: 4, 
	Source: "some-dialog", 
	Type: "Hide",
	IsManual: true,
	IsMinimized: false
}
```

### Type: Dismissed
* Triggers when the user tries to dismiss the dialog by clicking empty spaces on the screen.

### Type: Click
* Triggers when a button of a dialog is clicked by the user.
* The 'Source' field will now be the `${Name of the Dialog}-${Index of the Button}`.
* For example, if the first button of the "Play" dialog is clicked, the source field will be "Play-0".
* Parameter Dialog: Name of the operating dialog.
* Parameter Button: Index of the source button in the dialog.

### Type: Trigger
* Triggers when an element of <trigger=$source>Some text for the trigger</trigger> is clicked by the user.
* The 'Source' field will now be the $source defined by the element.
* To implement a glossary-like feature, try to put all items together in one section, activate it by default, and add event handlers for each keyword.
* Parameter Dialog: Name of the operating dialog.

### Type: Submit
* Triggers when the user submits an input to a question field.
* The 'Source' field will be the identifier of the input, which could be shared among multiple dialogs.
* Parameter Dialog: Name of the operating dialog.
* Parameter Content: Content of the input.
* Parameter Share: Whether the user agrees to share the answer with other users.

## Turtle Events (Category: 5)

## Patch Events (Category: 6)
 
## Link Events (Category: 7)

## NetTango Events (Category: 8)
### Type: Expand
* Triggers when the NetTango card is expanded by the user's interaction.

### Type: Shrink
* Triggers when the NetTango card is shrinked by the user's interaction.

### Type: Activate
* Triggers when a NetTango blockspace is activated by the user's interaction.

## Code & Command Center Events (Category: 10)
### Type: Execute
* Triggers when the user executes something in the command center.
* Parameter Source: Source of the execution.
* Parameter Code: Original (unwrapped) code of the execution.