# 📟 WinGUI

A graphical user interface library for Starfall, based on Windows 95.

## Documentation

You can find a list of all panel classes and their properties on [the wiki page](https://github.com/StyledStrike/starfall-wingui/wiki).

## Quick start

- Clone or download a .zip of this repository [here](https://github.com/StyledStrike/starfall-wingui/archive/refs/heads/main.zip)
- Extract the .zip and/or look for a file named `wingui.txt`
- Move `wingui.txt` to your Starfall data folder, which can be found at `YourSteamLibrary\common\GarrysMod\garrysmod\data\starfall`
- Copy the following test code, and paste it on a new tab on your Starfall editor
- Spawn the chip, and you should be greeted by a window with a button you can click on.

```lua
--@name WinGUI Quick Start
--@client
--@owneronly

--@include wingui.txt
local WinGUI = require( "wingui.txt" )

enableHud( nil, true )
input.enableCursor( true )

local function CreateWindow()
    -- create a test window
    local window = WinGUI:Create( "Window" )
    window.w = 300                  -- make it a wider
    window.h = 200                  -- make it a taller
    window:SetTitle( "My Window" )  -- set the text shown in the title bar
    window:Center()                 -- put the window in the middle of the screen
    window:MakePopup()              -- focus on this window

    -- add a test button to the window
    local button = WinGUI:Create( "Button", window )
    button.text = "Click me!"

    -- make the button fill the window's width
    -- and pinned to the top
    button:Dock( WinGUI.DOCK.TOP )

    -- listen to click events
    button.OnClick = function()
        print( "I was clicked!" )
    end
end

-- we'll need the game screen resolution
-- before we can create a window
local screenW, screenH

hook.add( "drawhud", "", function()
    if not screenW then
        screenW, screenH = render.getGameResolution()

        -- setup the desktop position and size on your HUD
        WinGUI:ConfigureDesktop( 0, 0, screenW, screenH )

        -- then create our window
        CreateWindow()
    end

    WinGUI:Render()
end )
```

## Starfall Screen Example

If you want to use WinGUI on regular Starfall screens instead of your HUD, you can use [WinGUI:EnableScreenMode](https://github.com/StyledStrike/starfall-wingui/wiki/WinGUI#winguienablescreenmode) like this:

```lua
--@name WinGUI Screen Example
--@client

--@include wingui.txt
local WinGUI = require( "wingui.txt" )

-- Makes so WinGUI can work on Starfall screens. Please note that
-- this will force you to use chat for text input, and
-- it persists until the chip is removed.
WinGUI:EnableScreenMode()

-- Since WinGUI already knows the desktop resolution,
-- you dont have to get the game resolution before
-- creating panels (like on the HUD example).

-- create a test window
local window = WinGUI:Create( "Window" )
window.w = 300                  -- make it a wider
window.h = 200                  -- make it a taller
window:SetTitle( "My Window" )  -- set the text shown in the title bar
window:Center()                 -- put the window in the middle of the screen
window:MakePopup()              -- focus on this window

-- add a test button to the window
local button = WinGUI:Create( "Button", window )
button.text = "Click me!"

-- make the button fill the window's width
-- and pinned to the top
button:Dock( WinGUI.DOCK.TOP )

-- listen to click events
button.OnClick = function()
    print( "I was clicked!" )
end

hook.add( "render", "", function()
    WinGUI:Render()
end )
```
