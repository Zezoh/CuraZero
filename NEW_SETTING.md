# How to add a new setting in Cura Zero #
If you want to see complete examples, see pull requests #71 (radio-box), #80 (text-input, bicolor, post-process), or #108 (check-box).
Under each step, there is a permalink to a line code. It shows the place to write the new line code.

If your setting is affected by bicolor printing, you can use this line to execute specific actions for bicolor printers : ```if int(profile.getMachineSetting('extruder_amount')) == 2```.

If necesary, don't forget to delete the files ```current_profile.ini```, ```mru_filelist.ini``` and ```preferences.ini``` to reset the settings of the app.

## Profile Setting (profile.py) ##
If your profile setting isn't implemented yet, your can create it or modify it in `Cura/util/profile.py` near the settings definitions.
If your setting value only depends on the printer, you can add it to the XML files in `resources/xml/`. If your profile setting has the same name, it will be automatically updated.

## User Interface (mainWindow.py) ##
1) Create a function ```init...``` to declare your widget and set a default value in `Cura/gui/mainWindow.py`.
2) Call this function in ```loadConfiguration``` in `Cura/gui/mainWindow.py`.
3) Add your widget to the window (the place of the line is important) in `Cura/gui/mainWindow.py`.
4) Create the refresh function. There are several ```putProfileSetting``` functions depending on the type of the setting (bool, float, ...) in `Cura/gui/mainWindow.py`.
5) Create the function called by the widget (not binded for the moment). Don't forget ```self.updateSceneAndControls(event)``` at the end in `Cura/gui/mainWindow.py`.
6) Then bind this function with the widget. The name of the event depends on the type of widget in `Cura/gui/mainWindow.py`.

The following steps depends on what you want to do with your setting, and what is already implemented.
The next sections shows different possibilities.

## Applying the setting (profile.py) ##
If necesary, you can modify the ```settings``` dictionary depending on the value of your profile setting, using ```profile.getProfileSetting```, ```profile.getMachineSetting``` or ```profile.getPreference``` depending on your settings's category in `Cura/util/sliceEngine.py`.

## G-Start (XML) ##
If you must use your new setting in the G-start (or the G-end), you can simply write it between curly braces ```{...}```. It will be automatically replaced in the final G-code in the printer XML files under `resources/xml/`.
If you want to write something more complex based on your new setting, like a complete sentence or the result of a calculation, you can write something between hash signs ```#...#``` and replace it in post-process (see section below).

## Post-process (sceneView.py) ##
To have a clearer code, you can capture the profileSetting in a variable before using it in `Cura/gui/sceneView.py`.
If you must replace something in the G-start, use the ```replace``` function on ```block0``` in `Cura/gui/sceneView.py`.
If you must modifiy or add something directly in the G-code, you can use the ```find``` function and write or rewrite a line with your new setting in `Cura/gui/sceneView.py`.

## Debug log (profile.py) ##
If necesary, you can add a message to help debugging in ```printSlicingInfo``` in `Cura/util/profile.py`.
