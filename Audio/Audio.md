## FMOD
Audio is managed with FMOD middleware using this [GDExtension](https://github.com/utopia-rise/fmod-gdextension).

The FMOD Studio project is included in the repo in the fmod/ subfolder and it builds bank files in godot/audio/fmod banks/Desktop.
### Autoloads
- **FmodManager** interacts with the FMOD API
- **GlobalFmodBankLoader** loads the prebuilt FMOD bank files from the path specified in Advanced Settings > Fmod > General
<img width="723" alt="image" src="https://github.com/user-attachments/assets/ce978e90-113f-4856-b5d9-9caa3b1a49d6" />

### Listeners and Event Emitters
The FMOD extension adds custom FmodEventEmitter and FmodListener nodes for audio which replace Godot's built-in AudioListener and AudioStreamPlayer nodes - they use some of the same methods like `play()` and `stop()`.
### Parameters
FMOD handles dynamic audio using **parameters** within the FMOD Studio project. These can be controlled through code in Godot:
- Local parameters (usually specific to one sound) are set on an FmodEventEmitter node using `set_parameter()`
- Global parameters are set through the FmodServer singleton using `FmodServer.set_global_parameter_by_name()`

The methods used here can vary slightly for labelled parameters which use discrete values

e.g. a Surface Type parameter which might be set to specific strings like `"snow"` or `"grass"` instead of a number
