# WebQuake-Stripped

This is a fork of WebQuake (Find the original README below). It makes some stuff work better for itch.io and adds empty resources. This is such that you can simply add a custom level ( `Client/id1/maps/level1.bsp` by default) and play it out of the box without needing any of Quake's resources. In this default configuration, shooting is disabled and there are no enemies, items, etc. Effectively, the game works as a walking simulator. To enable that stuff again, simply edit or replace the appropriate resources (in `Client/id1`) as described below:

- `default.cfg` is a text file containing the default controls and automatic level loading. Enable shooting there.
- `config.cfg` and `autoexec.cfg` should just be left empty and `quake.rc` probably unmodified.
- `gfx.wad` contains UI sprites. Edit with e.g. [AdQuedit](https://valvedev.info/tools/adquedit/) (source images have to be .pcx).
- `progs.dat` contains compiled Quake game play code. Here, the [Quake 1.06 qc source](http://icculus.org/twilight/darkplaces/files/id1qc.zip) compiled with [qcc](https://github.com/hypn/quake-qcc-binaries) is supplied.
- `gfx/` contains more UI sprites. Edit with e.g. [AdQuedit](https://valvedev.info/tools/adquedit/) (source images have to be .pcx).
- `maps/` should contain maps. Use e.g. [TrenchBroom](https://trenchbroom.github.io/) to make those.
- `music/` can contain `quake01.ogg` which is played as background music.
- `progs/` contains 3D models (`.mdl`) and 3D sprites (`.spr`). Export 3D models with e.g. [this blender add-on](https://github.com/victorfeitosa/quake-hexen2-mdl-export-import) and edit 3D sprites with e.g. [AdQuedit](https://valvedev.info/tools/adquedit/).
- `sound/` contains sound effects.

You can find non-empty, free resources in e.g. [LibreQuake](https://github.com/MissLav/LibreQuake) or use the proprietary assets from the full or shareware version of Quake itself as described in the WebQuake README below.


# WebQuake

**WebQuake** is an HTML5 WebGL port of the game Quake by id Software.

[Online demo by SpiritQuaddicted](http://quaddicted.com/forum/viewtopic.php?pid=438).

# Installing and running

Follow these steps to install WebQuake:

1. Install a web server with HTTP 1.1 Range support. The project was developed and tested on [Abyss Web Server](http://www.aprelium.com/abyssws/) on Windows, so it's guaranteed to work on it.
2. Download entire contents of the Client folder (index.htm and WebQuake/ folder) from the Code tab and put it somewhere on your server.
3. Get Quake resource files. The demo version containing only the first episode is enough.
4. Copy the `id1` folder from the Quake folder to the folder where you put index.htm.
5. If you have Quake mission packs, repeat step 4 for `hipnotic` and/or `rogue` folders.
6. If you're running a system with case-sensitive names (such as Linux), make sure that all game files **except for code files** have lowercase names.

To launch WebQuake, go to the WebQuake directory on your server in your browser.

To launch the game with command line arguments, add ? after the address and put the arguments after it in the same format as you use for Quake.

For Scourge of Armagon, add `-hipnotic` command line argument. For Dissolution of Eternity, add `-rogue`.

To launch mods, copy the mod folder into the folder containing index.htm and add `-game MOD_NAME_HERE` command line argument. Ensure step 6 of the installing instructions for the mod folder.

To use browser hotkeys (such as F5, Ctrl+T and Ctrl+W), click the address bar, and when you're done, click the game.

# Playing multiplayer

If you want to join a multiplayer game, do one of the following steps, go to Multiplayer menu in the main menu, type the IP in "Join game at" field and press Enter, or type `connect ws://ip:port` in the console.

You can also play WebQuake games in a native Quake (not QuakeWorld) client such as WinQuake or GLQuake. Choose TCP/IP in the Join Game menu.

You cannot join multiplayer games if the client is installed on https:// protocol, however.

If you want to create a server, first, install the dedicated server by completing the following steps.

1. Install [Node.js](http://nodejs.org).
2. Download the "Server" folder from the repository.
3. Put Quake resource files into the downloaded Server folder.
3. Open Node.js command prompt.
4. Go (cd) to the Server folder.
5. Type `npm install websocket` (for more information, see [Worlize/WebSocket-Node](https://github.com/Worlize/WebSocket-Node) repository).

Then, to launch a server, open Node.js command prompt, go to the Server folder and type `node WebQDS.js`.

To change maximum number of players, use `-maxplayers` command line argument.

## Remote console

To execute console commands on the server from the client or the web, set `rcon_password` in the server console. If you have spaces in the password, surround it with quotes.

Don't tell the password to anyone except for the server admins. **Don't put the password in the command line, as everybody on the web can see your command line on your server's `/rule_info` page!**

Then, you have 4 ways to execute server commands:

* In the game, when not connected, in the console, type `rcon_address ip:port` (without ws://), `rcon_password server_RCON_password` (surround the password with quotes if you have spaces in it), and then execute the commands by typing `rcon your_command_here`.
* In the game, when connected to the server, do the same as in the previous way except for settings `rcon_address`.
* Go to the server IP in the browser (for example, if your server is at `ws://192.168.0.2:26000`, go to `http://192.168.0.2:26000`). On the Rcon line, enter your command in the left field and the password in the right field and press Send.
* Go to `http://ip:port/rcon/your_command_here` in the browser. Login as "quake" with your RCON password.

## Server info API

You can retrieve some server information in JSON format by going to special addresses on your server IP.

* `/server_info` — returns an object containing the server name `hostName`, current level name `levelName`, number of connected players `currentPlayers`, maximum number of players `maxPlayers` and API version `protocolVersion`. Gives 503 when server is off.
* `/player_info` — returns an array of objects with the info about a player, where # is player number starting from 0. Contains name `name`, shirt/pants color `colors`, shirt color is upper 4 bits, pants color is lower 4 bits), number of kills `frags`, time since connected `connectTime` and IP address `address`. Gives 503 when server is off or 404 if the player is not found.
* `/player_info/#` — returns single player info object for the player under the number #.
* `/rule_info` — returns an array of all server console variables (like movement variables), in `{rule:"variable name",value:"variable value"}` format.
* `/rule_info/variable_name` — returns single server console variable in the same format. 404 if the variable doesn't exist.

# Adding game music

To add music to WebQuake, you need to get the music off the Quake CD and convert it into .ogg format ([Audacity](http://audacity.sourceforge.net/) is great for this).

The .ogg files should be called `quake##.ogg`, where ## is CD track number minus 1 with trailing 0, so the main theme is named `quake01.ogg` and the last track on the Quake disc is `quake10.ogg`.

Then you should configure the server to return audio/ogg MIME type for .ogg files.

After that, create "media" folder in the "id1" folder (or, for the mission pack music, "hipnotic" or "rogue") and put the .ogg files into it.

# Browser support

The port has been tested on the following browsers (results from 2013):

* Firefox (Windows) — **Very good** — developed on it.
* Chrome (Windows) — **Very good** — no "loading" image.
* Firefox (Android) — **Okay** — very low performance (canvas is locked at 12 FPS), no mouse support. Keypresses are incorrect, not tested with Windows keyboard.
* Chrome (Android) — **Not Good** — no "loading" image, sound is broken (launch with `?-nosound -nocdaudio`), no mouse. Requires Windows-compatible keyboard for Esc and F1-F12 keys.
* Opera (Windows) — **Not Good** — low performance, nothing is drawn in water (type `r_waterwarp 0` in the console), no mouse.
* Internet Explorer (Windows) — **Unsupported** — bad TypedArray support, but likely many more issues.

Mouse support is currently available only on Chrome and Firefox. Stereo positional audio is supported on Chrome and Safari.

# Tips

If the sound randomly doesn't play, go to console (press ~ or Options > Go to console in main menu), type `stopsound` and press Enter.

You can delete saved games by pressing Del in the load or save menus. This only works for the saved games created in WebQuake.

# Contributing

If you want to contribute to WebQuake, feel free to fork the project and make a pull request. Pull requests and issues will be reviewed by the developer.
