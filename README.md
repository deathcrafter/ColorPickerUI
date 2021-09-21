# ColorPickerUI

An ColorPicker for skin authors with a lot of options. Happy skinning!

https://user-images.githubusercontent.com/77834863/130698700-22820e92-09ed-4825-bda5-6055dae09a88.mp4

## Usage Instructions:

### Initialization
- `Creating a Measure`:

  First in the config you want to use it create a script measure and include the `ColorPickerUI.lua` in the ColorPickerUI folder. So basically it should look like this:
  ```ini
  [ColorPicker]
  Measure=Script
  ScriptFile=#ROOTCONFIGPATH#ColorPickerUI\ColorPickerUI.lua
  ; The above file path should direct to where you have kept the folder.
  ```
 
- `Dependencies`:
  
  Then include the **_SegoeIcons.ttf_** font file in `@Resources\Fonts`. You can find the file in resources folder of example skin or Fonts folder inside ColorPickerUI directory.

### Options:
- `OnFinishAction`:
  
  Bangs defined in this option are executed after user picks a color.
- `OnDismissAction`:
  
  Bangs defined in this option are executed if the user chooses to dismiss the picker using dismiss button. Force unloading using skin context menu would not trigger this.
- `Theme`:
  
  Which theme to use. In-built themes include Dark and Light. Follow tips and tricks in the end to know how to create and use new themes.
- `Animations`:
  
  Whether to use initial animation.

### Temporary Variables:
  These variables would not persist after refresh or the addon config is used once again. These can be used in bangs defined in Actions defined inside the measure after color is chosen or addon is dismissed.
- `$color$`:

  This is replaced by the last color choosen by an user. Not avaialable for **_OnDismissAction_**.
- `$option$`:

  This option is replaced by the option, or variable name user chose to change.
- `$section$`:

  This is replaced by the section where the option belongs to, "_Variables_" for a variable.
- `$filePath$`:

  This is replaced by the file name where the color value is supposed to be written. Default value is the path to the current config's file path(_#CURRENTPATH##CURRENTFILE#_).
  
### Custom Action:
This is an optional functionality, but very useful.
- `ColorChangeAction`:

  Contrary to other options above in `Options` section, this option can be defined in any section, measure or meter. This action can be set when the function to activate the addon is called. The bangs defined in this option can also use temporary variables mentioned above. This action, if defined, will be executed __before *OnFinishAction*__.

### Activation:
- `SetColor('option', 'section', 'format', 'fileName', 'actionSection')`:
  This is the function which is to be called using "**_!CommandMeasure_**" to set a color. Argumaents described below:
  1. **option:**(required) The option which to change, variable\_name for variables.
  2. **section:**(required) The section where the _option_ is in. "**Variables**" for variables.
  3. **format:**(optional, default: rgb) In which format the color will be written. Available formats are:
      - rgb[a] - 255,255,255[,255]
      - hex[a] - FFFFFF[FF]
      - hsv[a]\*\* - 0,0,1[,1]
      - hsl[a]\*\* - 0,0,1[,1]
  - \*\*Though available, not supported by Rainmeter. **Using _rgb, rgba, hex_ or _hexa_ recommended**.
  4. **filePath:**(optional, default: current filepath) The file where the _section_ is in or where value is supposed to be written.
      - **Important**: ```You must use [[filePath]] to enclose file paths. For example [[#ROOTCONFIGPATH#Dependencies\Variables.inc]].```
  5. **actionSection:**(optional, no default) The section where the [ColorChangeAction](https://github.com/deathcrafter/ColorPickerUI#custom-action) is defined. The bangs will be executed after and only if a color is choosen, and before OnFinishAction.
  
  The arguments must be in order and optional arguments must not be skipped if the next argument is to be used. Only file name can be an empty string(**''**) to use default value.
- `Example`:
```ini
[SomeMeter]
SolidColor=0,0,0
LeftMouseUpAction=[!CommandMeasure ColorPicker "SetColor('SolidColor', 'SomeMeter')"]

; addon will write the value of SolidColor option under SomeMeter in rgb format.
```
```ini
[SomeOtherMeter]
FontColor=FEFEFEFF
LeftMouseUpAction=[!CommandMeasure ColorPicker "SetColor('FontColor', '#CURRENTSECTION#', 'hexa')"]

; addon will write the value of SolidColor option under SomeMeter in hexa format.
```
```ini
[AnotherMeter]
ImageTint="#ImageTint#"
LeftMouseUpAction=[!CommandMeasure ColorPicker "SetColor('ImageTint', 'Variables', 'rgba', [[#@#Variables.inc]])"]

; See that I enclosed #@#Variables.inc with `[[]]` and not `''`. It is important since lua considers `\` as escape sequence.
; addon will write the value of variable ImageTint in rgba format located in the file Variables.inc in @Resources
```
```ini
[FirstMeter]
FontColor=0,0,0,200
LeftMouseUpAction=[!CommandMeasure ColorPicker "SetColor('FontColor', 'FirstMeter', 'rgba', '', 'SecondMeter')"]
Group=Meters

[SecondMeter]
FontColor=0,0,0,200
ColorChangeAction=[!SetOptionGroup Meters $option$ "$color$"][!UpdateMeterGroup Meters][!Redraw][!WriteKeyValue SecondMeter $option$ "$color$"]
Group=Meters

; addon will first write the value of FontColor under FirstMeter.
; In the next step it will execute the bangs in the ColorChangeAction defined in 'SecondMeter' after replacing the temporary variables.
; Now it will SetOption and Redraw so you don't need to refresh. Then it will permanently write the value to "SecondMeter" too.
; Also notice that I used '' to skip the fileName parameter. This is only applicable in case of fileName.
```

### Themes:
- You can create a new inc file inside `ColorPickerUI\Themes` and name it what you want. Then copy the contents of existing themes and edit the color variables. In the measure, use `Theme=your_theme_name.inc`. That's all.

## Tips and Tricks:
### Using Windows theme to switch between Dark and Light theme:
```ini
[WindowTheme]
Measure=Registry
RegHKey=HKEY_CURRENT_USER
RegKey=Software\Microsoft\Windows\CurrentVersion\Themes\Personalize
RegValue=AppsUseLightTheme
Substitute="1":"Light", "0":"Dark"
```

## Credits:
- **JSMorely** for **CursorColor Plugin**
- **NightHawkSLO** for **Mouse Plugin**

## Bug Reports:
This being a new skin and a complex one, bugs might spring up. Please open an issue here and let me know. Thank you.😀
