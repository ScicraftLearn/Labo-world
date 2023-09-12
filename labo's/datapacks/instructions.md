# Datapacks

## Extensions

There are 2 types of extensions used for datapacks.
1) **.mcfunction**: for specifying the functionality of the datapack as a list of commands that the game will execute.
2) **.json**: for writing advancements or loot tables.

## Building a datapack folder structure

1) Create a folder (it doesn't matter where) and give it a name.
2) In this folder, create a **pack.mcmeta** file with the following content:
   ```json
    {
       "pack": {
           "pack_format": 10,
           "description": "Description of the datapack."
       }
    }
   ```
    * Pack format indicates the version of the game that the datapack is compatible with.
        * mc_version: 1.19 -> pack_format: 10
        * mc_version: 1.18.2 -> pack_format: 9
        * ...
3) Create the following folder structure inside the folder you created:
    ```
    Folder/
    |__ pack.mcmeta
    |__ data/
        |__ namespace/
            |__ functions/
                |__ Folder2/
    ```
    * The _data_ and _functions_ folders must keep their names. The _namespace_ and _Folder2_ folders can be named differently.
4) Inside the _Folder2_ folder, create a **.mcfunction** file that will contain the functionality.

## Installing & testing

1) Zip the **pack.mcmeta** file and the _data_ folder.
2) Move the zip file to the datapacks folder located in the Minecraft world folder.
3) Start the world, if it's already running, you can use the /reload command to load the datapack.
4) Execute the command /function namespace:Folder2/function_name to check if the function works.

### For testing

You don't need to make a zip everytime, you can just place the main folder in the datapacks folder and execute the /reload command after making changes in the files.

## Load & tick event

* Load event: Function will be executed when the world is loaded.
* Tick event: Function will be executed every tick.
    * May cause lag if many functions are executed simultaneously.
    * Conditions can be added to avoid invoking a function every tick.

To implement this, modify the folder structure to look like this:
```
Folder/
|__ pack.mcmeta
|__ data/
|   |__ namespace/
|       |__ functions/
|           |__ Folder2/
|
|__ minecraft/
    |__ tags/
        |__ functions/
```

Create 2 new .mcfunction files and place them in Folder2:
* load.mcfunction
* tick.mcfunction

These files will contain the commands to be executed on load or per tick.

To make this work, you need to add 2 files to the functions folder located in the tags folder:
* load.json
* tick.json

The **load.json** file should contain the following content:
```json
{
  "values": ["namespace:Folder2/load" ]
}
```

The **tick.json** file should contain the following content:
```json
{
  "values": ["namespace:Folder2/tick" ]
}
```

The final structure will look like this:
```
Folder/
|__ pack.mcmeta
|__ data/
|   |__ namespace/
|       |__ functions/
|           |__ Folder2/
|               |__ some_funcion.mcfunction
|               |__ load.mcfunction
|               |__ tick.mcfunction
|
|__ minecraft/
    |__ tags/
        |__ functions/
            |__ load.json
            |__ tick.json
```