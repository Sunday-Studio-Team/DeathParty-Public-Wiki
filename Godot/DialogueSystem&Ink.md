# Table of Contents
1. [Overview/Basic Dialogue Box Setup](#overview)
2. [Custom Choice Buttons](#custom-choice)
3. [Custom Dialogue Lines](#custom-line)
4. [Creating New Characters](#character-properties)
5. [Connecting to Ink/Starting a Dialogue](#connecting-to-ink)

<a id="overview"></a>
## Overview/Basic Dialogue Box Setup
The **DialogueSystem** is intended to give as much flexibility as possible to designers in terms of layout and style of a dialogue box. Everything is `Resource`-based. Want to define a new dialogue box, character or choice line? Use a `Resource` (see below.)

A dialogue box consists of a few key elements that determine its basic functionality:
1. The root of the dialogue box scene must be a **Control** node with the script `dialogue_box_properties.gd`. This will allow you to assign properties through the inspector. Save this as a scene when you are done.
    * `Dialogue Container`: a **VBoxContainer** that will contain dialogue text.
    * `Choice Container`: a **VBoxContainer** that will contain dialogue choices.
    * _(Optional)_ `Image Container`: a **TextureRect** that will display the image of the speaker.
    * _(Optional)_ `Name Container`: a **RichTextLabel** that will contain the speaker's name. If this is not provided, the speaker's name may be prefixed to any dialogue they say (i.e. CREEPY PIKACHU: Hello!)
    * _(Optional)_ `Resource ID`: an **int** that refers to the index of the `Resource` file corresponding to this dialogue box. ***You only need this if you don't want DialogueSystem to handle the instantiation of your dialogue box (such as passing control to a pre-existing dialogue box.) Otherwise, DialogueSystem handles this for you.**

2. Create your dialogue box's `Resource` file. Navigate to `res://Assets/Resources/DialogueBoxResources/Default Resource (DO NOT EDIT)/` and create a copy of the default file labeled `dialogue_box_properties.tres`. Move this copy into the `DialogueBoxResources` folder. Below are the properties you can set using `dialogue_box_properties.tres`. Most of these have default values or are optional!

|Property                        | Default Value    | Type      | Purpose
| ----                         | ----:            | ----:     | ---
|dialogue_box | null | `PackedScene` | **IMPORTANT!** The scene you created above for your dialogue box's layout.
|dialogue_line | `diag_line.tscn` | `PackedScene' | The scene that you will be using to display dialogue text added to the Dialogue Container.
|choice_button | `choice_button.tscn` | `PackedScene' | The scene that you will be using to display choices added to the Choice Container.
|text_font | null | `FontFile` | (Optional) The font for dialogue text.
|name_font | null | `FontFile` | (Optional) The font for names (such as speaker names.)
|text_size                     | 15               | `int`       | Sets font size of dialogue text.
|name_size                     | 15               | `int`       | Sets font size of name text.
|choice_size                   | 15               | `int`       | Sets font size of choice text.
|default_text_color            | "white"          | `String`    | Sets default font color of text through BBCode. This is overwritten if a character has a specific color assigned to them in `Character Properties` (see below.)
|default_name_color            | "yellow"         | `String`    | Sets default font color of speaker name's text through BBCode. Overwritten by `Character Properties`, if set.
|default_choice_color          | "yellow"         | `String`    | Sets default font color of choice text through BBCode.
|line_separation | 0 | `int` | How much separation between lines of dialogue text.
|include_speaker_in_text       | true             | `bool`      | When you don't have a dedicated `Name Container` (see above), this property will decide whether the speaker's name will included in the text (i.e. "CREEPY PIKACHU: Hello!" vs "Hello!")
|prefix_choices_with_player_name | true             | `bool`      | Similar to `include_speaker_in_text`, this property decides whether to include "YOU:" in front of any lines said by the player.
|clear_box_after_each_dialogue | false            | `bool`      | Decides whether the dialogue box will be cleared with each new dialogue, or if it will keep dialogue history.
|text_animation                | "typewriter"     | `String`    | Choose between `"typewriter"` or `"sliding"` for your text animation. For no animation, set to `"none"`. _("sliding" is currently not available)_
|image_key                     | "full"           | `String`    | Most characters will have multiple images assigned to them in `Character Properties` Choose between `"full"` (full body) and `"torso"` (waist-up) to decide which will be displayed in your dialogue box.

3. No need to register your `Resource` anymore. Just make sure it is stored in the `DialogueBoxResources` folder.

### Using your dialogue box ingame
To instantiate and use your dialogue box ingame, use the following command: 

```DialogueSystem.setDialogueBox(<Resource>)```

To use an existing GUI Control (assuming it is correctly configured with `dialogue_box_properties.gd`) as your current dialogue box, use the following command: 

```DialogueSystem.transferDialogueBox(<Control>)```

To start dialogue, pass a JSON file through this command:

```DialogueSystem.from_JSON(EXAMPLE_JSON_FILE)```

**If you are using `transferDialogueBox()` to transfer control to an existing dialogue box `Control`, make sure you define your dialogue box's Resource File in `dialogue_box_properties.gd`.**

### Example Dialogue Box Hierarchies

Notice that the structure doesn't really matter as long as there are defined containers for dialogue, choices and image. **You can also set the dialogue and choice containers to be the same container.** This gives you a lot of leeway on how to design your box!

![hierarchy_1](https://github.com/user-attachments/assets/59ca49b6-8b0e-4882-839b-f6e59909923b)
![hierarchy_2](https://github.com/user-attachments/assets/acf75b96-9d0e-4921-aa4e-6ed3ba804ba8)
![hierarchy_3](https://github.com/user-attachments/assets/de44fb0c-4da0-4f92-8420-e6cf1edbd1c3)

<a id="custom-choice"></a>
## Custom Choice Buttons
If you want to be really fancy, you can even customize the layout of your choice buttons. WARNING: figuring out the layout can be really messy and time-consuming.

1. Attach `choice_button_script.gd` to your root node (Must be `Control`-derived. I recommend a MarginContainer so you can easily set gaps between other buttons, and automatically resize according to the contents inside it)
2. You need a `RichTextLabel` for your choice text, and a `Button` to allow the user to select the choice. Assign these through your root node in the inspector.
3. Once you have your custom choice button saved as a scene, assign it to your dialogue box's root node through its `Choice Button` property!

### Example Choice Button Hierarchies

![button_hierarchy_1](https://github.com/user-attachments/assets/9f3695db-5e0e-428a-bd15-7ca31fec74f2)
![button_hierarchy_2](https://github.com/user-attachments/assets/b985e45f-9980-4dac-9361-327fe5e4c611)

<a id="custom-line"></a>
## Custom Dialogue Lines
1. Attach `dialogue_line_script.gd` to your root node (Must be `Control`-derived. I recommend a MarginContainer.)
2. You need a `RichTextLabel` for your dialogue text, and optionally a `RichTextLabel` for the speaker's name. Assign these through your root node in the inspector.
3. Once you have your custom dialogue line saved as a scene, assign it to your dialogue box's root node through its `Dialogue Line` property!

<a id="character-properties"></a>
## Creating New Characters
Similar to dialogue boxes, characters are registered using `Resource`s.
1. Duplicate the default character resource at `res://Assets/Resources/CharacterResources/Default Resource (DO NOT EDIT)/character_properties.tres`. **IMPORTANT:** Put your resource in the `CharacterResources` folder.
2. Edit it as you please! A property table is provided below.
3. No need to register your `Resource` anymore. Just make sure it is stored in the `CharacterResources` folder.

When your character speaks, DialogueSystem will automatically refer to their `Resource` file. As we continue development, we will likely add support for further properties!

|Property | Default Value | Type | Purpose
| ---- | ----: | ----: | ----
|name | "" | `String` | Your character's name (case sensitive.)
|image_full | null | `CompressedTexture2D` | Image of your character, full body.
|image_torso | null | `CompressedTexture2D` | Image of your character, waist up.
|name_color | "" | `String` | Name color that overrides default dialogue box properties when this character is speaking. Set through BBCode.
|text_color| "" | `String` | Text color that overrides default dialogue box properties when this character is speaking. Set through BBCode.

<a id="connecting-to-ink"></a>
## Connecting to Ink
Your dialogue should be exported from [Ink](https://www.inklestudios.com/ink/) in JSON format. In order to start a dialogue with your Ink file, use the following commands:

```
@export var EXAMPLE_JSON_FILE : JSON
DialogueSystem.setDialogueBox(<your_dialoguebox_Resource>)
DialogueSystem.from_JSON(EXAMPLE_JSON_FILE)
```

You can run this code upon pressing "E" on a character, for example. Don't forget to set your preferred dialogue box beforehand, or it will error!

The variables you declare on Ink are stored **globally**. **They can be accessed and modified using `SaveSystem`!!!** For example, say you have the following Ink dialogue:

![ink_example_image](https://github.com/user-attachments/assets/5d6b0306-5635-497d-8bc3-b1857d56f25c)

As long as `set_this_to_true` is false, we won't see the second option:

![one_option_example](https://github.com/user-attachments/assets/34d843a2-64ba-404a-94b9-a19b83084fb5)

However, the following code will overwrite `set_this_to_true`'s default value, allowing the option to appear.

```
SaveSystem.set_key("set_this_to_true", true)
DialogueSystem.from_JSON(EXAMPLE_FILE)
```

![two_option_example](https://github.com/user-attachments/assets/3f30502e-8193-4db0-82a1-87d0ec4cae4f)

We can use this to keep track of what stats the player has, what outfit they are wearing, etc.

It is important to note that if a variable has been previously defined elsewhere, declaring and initializing it at the beginning of a new dialogue in Ink **won't alter its value ingame**. That way, we ensure that we can transition from dialogue to dialogue without automatically resetting variables. If you want to edit a variable's value through Ink, please perform a separate operation on it.
