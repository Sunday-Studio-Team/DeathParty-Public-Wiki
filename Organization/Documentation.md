# Documentation
The documentation is two important parts. The documentation that is written here in the wiki, which contains a general overview of a part of the code you have worked on, and the actual code. Ideally the code is readable enough that it does not require documentation but due to how big the codebase might get and the fact we have several programmers, the wiki exists to provide guidance to jump into a specific area of the code.

## GitHub Wiki Documentation


## Styling Guide
Based on the Godot Styling Guide found : https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html

The styling guide helps ensure that your code matches the code that other people write. It's not super strict, but following it helps maintain a level of consistency.

### **Naming Convention**

| Type    | Convention | Example
| -------- | ------- | ------- |
| File names  | snake_case    | dialogue_logic.gd |
| Class Names | PascalCase    | class_name YAMLParser |
| Node Names    | PascalCase  | Camera3D PlayerCamera |
| Functions | snake_case |  func load_level():  |
| Variables | snake_case | var particle_effect |
| Signals | snake_case | signal door_opened |
| Constants | CONSTANT_CASE | const MAX_SPEED = 200 |
| Enum names | PascalCase | enum Element |
|Enum members | CONSTANT_CASE | {EARTH, WATER, AIR, FIRE} |

### **Example**
```
class_name StateMachine
extends Node
## Hierarchical State machine for the player.
##
## Initializes states and delegates engine callbacks ([method Node._physics_process],
## [method Node._unhandled_input]) to the state.

signal state_changed(previous, new)

@export var initial_state: Node
var is_active = true:
	set = set_is_active

@onready var _state = initial_state:
	set = set_state
@onready var _state_name = _state.name


func _init():
	add_to_group("state_machine")


func _enter_tree():
	print("this happens before the ready method!")


func _ready():
	state_changed.connect(_on_state_changed)
	_state.enter()


func _unhandled_input(event):
	_state.unhandled_input(event)


func _physics_process(delta):
	_state.physics_process(delta)


func transition_to(target_state_path, msg={}):
	if not has_node(target_state_path):
		return

	var target_state = get_node(target_state_path)
	assert(target_state.is_composite == false)

	_state.exit()
	self._state = target_state
	_state.enter(msg)
	Events.player_state_changed.emit(_state.name)


func set_is_active(value):
	is_active = value
	set_physics_process(value)
	set_process_unhandled_input(value)
	set_block_signals(not value)


func set_state(value):
	_state = value
	_state_name = _state.name


func _on_state_changed(previous, new):
	print("state changed")
	state_changed.emit()


class State:
	var foo = 0

	func _init():
		print("Hello!")
```
