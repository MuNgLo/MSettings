# MSettings v1.0

Static generic class to handle serialization and caching of configs.
File format JSON
Files stored under application/Settings by default.

# How to use
One easy way is to define a property where you want to access a config like this
```cs
    public static PlayerConfig Config => MSettings.Settings.GetSettings<PlayerConfig>("");
```
That will look for a json file called PlayerConfig under Application/Settings folder.
If there is no file. A new one will be made with all default values.
It reads file, deserialize it to the Type, cache it in a dictionary and returns it.

To avoid excessive file access make use of GetCachedSettings. Note that it will return null if it hasn't been cached into the dictionary.
```cs 
    public static PlayerConfig Config => MSettings.Settings.GetCachedSettings<PlayerConfig>("");
```

# Define the config
For serialization to work use attributes in the config class.

Example:
```cs
    [JsonInclude, MenuLabel("Name"), Tooltip("The name you will show up as in a game")]
    public string playerName = "ConfigDefaultName";

     [JsonInclude, Range(0.1f, 100.0f), MenuLabel("Mouse Speed")]
    public float mouseSensitivity = 10.0f;

    [JsonInclude, Range(30, 120), MenuLabel("Field of View"), Tooltip("Horizontal angle for camera viewport")]
    public int fov = 85;
```

For Godot.Color Type you can do this.
```cs
    [JsonInclude]
    public string playerColour = "ff5200";
    [JsonIgnore, MenuLabel("Colour"), Tooltip("Mainly colour of your name in chat")]
    public Color PlayerColour
    {
        get => Color.FromString(playerColour, Colors.White);
        set => playerColour = value.ToHtml();
    }
```

# Options menu prefab scene
Under the DropinScene folder there is a UISettingsEntryPrefab.tscn. Thgat is setup to be compatible with the MSettings way of doing things.
Just drop it in nad set the 2 values in inspector.
Settingsname should just be the config class name.
Field Target is the field/property it should expose.
Depending on the Type of the field/property, different UI Nodes will be shown under the prefab as needed.
Copying the prefab and making your own, better looking one is recommended.