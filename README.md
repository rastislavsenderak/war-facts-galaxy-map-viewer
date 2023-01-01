# war-facts-galaxy-map-viewer
3D galaxy map viewer for www.war-facts.com game

This is a "technical demo" or "proof-of-concept" for viewing 3D galaxy map of Warring Factions game (https://www.war-facts.com/).
It is not my intention to replace in-game map, just to add overall/global (static) view for whole galaxy (or galaxies) as a strategic view with enhanced visualisation of exploration info (explored/unexplored, my/ally/enemy colonies etc).

Uses https://threejs.org/ and its addons.

Tested on Windows 10 / Google Chrome browser (Version 108.0.5359.125 64-bit)

![Example map view](https://github.com/rastislavsenderak/war-facts-galaxy-map-viewer/blob/main/tuson-galaxy-example.gif)

## How to Install
Just download or clone or copy to some local dir. It's just 3 files, really:
* index.html - html and javascript code
* favicon.ico - just a favicon (optional)
* tuson-galaxy-example.json - example data file. It contains one quarter of real Tuson galaxy (from instance Renatus ex Cinere) with fake colony info. Provide your own file if you want to see your exploration data / your favorite galaxy. See bellow for technical/format details.

## How to Run
In theory you only need to open this **index.html** file (in your browser). But this example reads local data file (tuson-galaxy-example.json) and CORS (security) does not allow it in most modern browsers (duh!)
Therefore you need some http server - see https://threejs.org/docs/#manual/en/introduction/How-to-run-things-locally, or allow it in your browser (google for more info).

I have Python installed, so I just run
```
python -m http.server
```
in that local dir, and then type http://localhost:8000/ in my browser.

## Usage
Mouse controls - wheel for scrolling, left and rigth button with mouse moving to pan / rotate etc.
No more interactions, yet.

## Technical / dev notes
* I use Three.js version r124 (see imports). Higher versions changed THREE.Geometry - see https://github.com/tengbao/vanta/issues/101. Should be no problem to fix/upgrade it, though.
* Particle (THREE.Point) is used to display/render stars - and it works fine event with millions of stars. Labels are more tricky. See bellow.
* I've implemented two ways to show labels (see USE_HTML_LABELS and/or USE_SPRITE_LABELS in the code):
  ** as sprites (can be zoomed), but do not try do display millions of them (too slow)
  ** as html / text (2d) elements (can't be zoomed), but much faster, and it's html, so they can be styled, and seems bit more readable etc).
* Currently input data is taken from local file, in JSON format:
```
[{"ID": 7, "name": "?", "x": -17999, "y": 105366, "z": 26013},
{"ID": 8, "name": "Tuson-A8-G1-Central", "x": -65746, "y": 13292, "z": 4211, "tag": "E"},
{"ID": 13, "name": "Tuson-A8-G2-Core", "x": -156254, "y": 171194, "z": -37398},
{"ID": 15, "name": "?", "x": -25750, "y": 98257, "z": -34959, "tag": "M"},
...
```
* This data format is basicly result of API call https://beta1.war-facts.com/wiki/API ("All systems in #galaxy"), sometimes enriched with attribute "tag":
```
'M' = my colonies (yellow, with star name labels)
'O' = other colonies (red, with star name labels)
'E' = explored (white/grey, no labels)
none, or anything else = unexplored (blue, no labels)
```
* FPS can be adjusted, I use 30 or less (see ```startAnimating(30);``` call); real FPS is monitored using "stats" tool - see mini display in upper left corner.


## TODO
* Labels should hide if too far. Currently only one "far" plane is supported for camera, but I can fix it using differen layers for different renderes etc. Maybe soon.
* OrbitControls is not too convenient for this case (no free motion, just rotating around 0,0,0 point). I would use "flee-fly" camera or something similar.
* Add links - clicking on planet and/or label to show some info and/or redirect to game itself (show the system detail, for example).
* Add more (static) objects to the map - bookmarks/rallypoints etc. I do not plan to add dynamic objects like fleets, or more controls.
