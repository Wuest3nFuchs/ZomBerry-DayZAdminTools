# ZomBerry Admin Tools
ZomBerry is still in early testing stage, please report about any issues!
Don't forget to mention game version in issue description

## Usage
### Installation:
* Make sure that you have installed [RPCFramework](https://github.com/Jacob-Mango/DayZ-RPCFramework) by Jacob_Mango, Arkensor and Kegan Hollern
* Download latest release [here](https://github.com/Moondarker/ZomBerry-DayZAdminTools/releases)
* Unzip the file into the root of your game and server directory
* Add RPCFramework and ZomBerry to your game and server launch options. For example:  
```-mod=RPCFramework;ZomBerry;```

On server:
* Copy file ```Vaker.bikey``` from ```ZomBerry\Keys``` into server ```keys``` folder
* Copy file ```admins.cfg``` from ```ZomBerry\Config``` to ```[ServerProfilesFolder]\ZomBerry\``` subfolder. Default server profiles dir is ```%LocalAppData%\DayZ\``` (alternatives: default dir, custom dir (passed through  
```-zbryDir=path_to_dir``` launch option), mission dir (you might need to modify your mission ```init.c``` and add line   ```g_Game.SetMissionPath(path);``` in ```CreateCustomMission``` method)
* Add your BIGUID into ```admins.cfg``` file (use new line for each admin entry, you can easily find BIGUID in server logs)

On client:
* Copy file ```ZomBerry.cfg``` from ```ZomBerry\Config``` to ```C:\Users\[Your Username]\Documents\DayZ``` dir and change menuKey (if you wish to)

Default menu key is ```M```

### For developers:
ZomBerry was created to be as simple and lightweight for users and developers as possible. Right now, there are two main functions available:

**AddCategory** requires two arguments: 
* string Category name
* int color code (basically, colors are usual HEX color codes but with first byte used for alpha channel. Example: 0xFF00FF00 is opaque green color)
```java
GetZomberryCmdAPI().AddCategory("MyMod", COLOR_RED);
```

**AddCommand** requires four arguments and have two optional arguments: 
* string displayName - name shown in functions list
* string functionName - name of function which will be called
* Class instance - instance of the Class which the function resides
* string categoryName - name of category for function to be placed
* (optional) bool targetRequired - is target required to execute command? Default: true
* (optional) ZBerryFuncParamArray functionParams - array of custom parameters, check examples below. Default: {}
```java
GetZomberryCmdAPI().AddCommand("Induce sneeze", "MyModSneezeTarget", this, "MyMod", true);
```
Example with custom parameters:
```java
GetZomberryCmdAPI().AddCommand("Set time", "SetTime", this, "MyMod", false, {
  new ZBerryFuncParam("Hour", {0, 23, 12,}), //Params are: string paramName, TIntArray {minValue, maxValue, defaultValue}
  new ZBerryFuncParam("Minute", {0, 59, 0,}), //You may use up to 3 custom parameters, this example contains only 2
});
```

Your function will be called with five arguments:
* string functionName (explains itself)
* int adminId - session id of admin who executed this function 
* int targetId - session id of targeted player (=adminId if no target was selected)
* vector cursor - position where admin's cursor intersects with ground
* (optional) TIntArray cmdParams - array with 3 int values, each represents one of custom parameters (check examples below)  

Note: you may get PlayerBase of player's character using ```ZBGetPlayerById(int playerId)``` function

Example of CmdAPI interaction can be found [here](https://github.com/Moondarker/ZomBerry-DayZAdminTools/blob/master/ZomBerry/Addons/scripts/5_Mission/ZomBerryStockFnc.c) and [here](https://github.com/Moondarker/ZomBerry-DayZAdminTools/blob/master/Examples/missionExample.chernarusplus/init.c)

## Planned features and updates:
* Major UI tweaks and code review
* New functions in default kit

Done:
* Make commands list completely server-side, so it could be supplemented directly from mission/serverside-only addons
