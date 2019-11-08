 MV3D
======

A 3D rendering plugin for RPG Maker MV.

Make sure you have both the `three.js` plugin and `MV3D.js` plugin loaded, in
that order.

Now when you run your game, the map should be rendered in 3D.

The A3 and A4 tiles will be rendered as walls. You can also change the height
of tiles using regions and terrain tags.

By default, regions 1-7 are configured to affect the height.

Terrain tag 1 is configured to use a cross shape, so tiles with this tag will
stand up like a tree.

Terrain tag 2 is configured to use a fence shape. Try putting this tag on the
fence autotiles that come with MV.

The regions and terrain tags can be reconfigured however you want.

---

## Tileset Configuration

A more advanced feature, the tileset configuration should be placed in the
tileset's note, and should be wrapped in an <mv3d></mv3d> block.

Each line in the configuration should start by identifying the tile you want
to configure, followed by a colon, and then a list of configuration functions.

Choosing a tile is done with the format `img,x,y`, where img is name of the
tileset image (A1, A2, B, C, etc.), and x and y are the position of that tile
on the tileset. For example, `A2,0,0` would be the top left A2 tile. On the
outdoor tileset, this would be the grass autotile.

Each tile can have 3 different textures. The top texture, side texture, and
inside texture. The side texture is used on the side of the tile when it has a
positive height, and the inside texture is used when it has a negative height.

If inside texture isn't specified, all sides will use the same texture.  
If side texture isn't specified, then sides will use the top texture.

---

	texture(img,x,y)
	top(img,x,y)
	side(img,x,y)
	inSide(img,x,y)

These functions will use the specified tile for the current tile's textures.  
texture() sets both the top and side textures.

---

	offset(x,y)
	offsetTop(x,y)
	offsetSide(x,y)
	offsetInside(x,y)

These functions will use an offset to get a texture from a tile near this one.
These should only be used to point to tiles on the same tileset image.

---

	rect(img,x,y,w,h)
	rectTop(img,x,y,w,h)
	rectSide(img,x,y,w,h)
	rectInside(img,x,y,w,h)

These functions use a specified region from a tileset image as the texture.
They use pixel coordinates rather than tile coordinates.

---

	shape(s)

The shape function can set the tile's shape to FLAT, FENCE, CROSS, or XCROSS.
Each of which have their own behavior. FLAT is default.

---

	float(n)

The float function will make water vehicles float a certain height above the
tile.

---

	fringe(n)

The fringe function will move the tile into the air by the specified distance.
Usually used for tiles with star passability.

---

As a simple example, we'll give the water tile in the outdoor tileset a
negative height so it will sink into the ground.

    <MV3D>
      A1,0,0:top(A1,0,0),rectSide(A1,31,54,31,14),height(-0.3),float(0.1)
    </MV3D>

But this example doesn't look very good when we place it on the edge of a
cliff, so we can configure it to use waterfall textures on the outside walls.

    <mv3d>
      A1,0,0:top(A1,0,0),side(A1,1,1),rectInside(A1,31,54,31,14),height(-0.3),float(0.1)
    </mv3d>

---

## Event Configuration

Event configurations can be placed in either the note or comment.  
Event configurations should be contained in an <mv3d: > tag, where a list of
configuration functions goes after the colon.

---

	x(n)
	y(n)

The x and y functions shift position of the event's mesh.

---

	height(n)

The height function sets the height of the event above the ground.

---

	z(n)

The z function sets the z position of the event. Ignores ground level.

---

	pos(x,y)

The Pos function sets the position of the event.   
If in the note tag, position will only be set when the event is created.  
If in the comments, position will be set when event changes pages.  
Prefix numbers with + to use relative coordinates.  

---

	side(s)

Which sides of the mesh will be rendered. Can be FRONT, BACK, or DOUBLE.

---

	shape(s)

Sets the shape of the event's mesh.

Valid shapes:

 -   FLAT - Event lies flat on the ground like a tile.
 - SPRITE - Event rotations both pitch and yaw to face camera.
 -   TREE - Event stands straight up, rotating to match camera yaw.
 -  FENCE - Event stands straight up, facing south. (rotate using rot)

---

	scale(x,y)

Sets the scale of the event's mesh.


---

	rot(n)

Rotates the event. Only works with flat and fence shapes.  
0 is south, 90 is east, 180 is north, and 270 is west.  

---

	bush(true/false)

Whether the event is affected by bush tiles.

---

	shadow(true/false)

Whether the event has a shadow.

---

	shadowScale(n)

Set's the scale of the event's shadow.

---

	alphaTest(n)

Doesn't render any pixels below the alphaTest value.  
0 means all alpha will be rendered. 1 means no alpha.  
Can be useful for removing unwanted shadows in the textures.  

---

	lamp(color,intensity,distance)
	flashlight(color,intensity,distance,angle)

Sets up light sources on this event.  
Color should be a hex code.  
Intensity is the brightness of the light.  
Distance is how far the light travels.   
Angle is the width of the flashlight's beam.   

If configured on note, these settings are applied only when event is created.  
If in comment, settings applied when switching pages.

---

	flashlightYaw(deg)
	flashlightPitch(deg)

Sets the pitch and yaw of the event's flashlight.  
Prefix the yaw angle with + to set yaw relative to event's facing.

---

	lightHeight(n)

Sets the height of the light sources on the event.

---

As an example, to make a door event render on the edge of a building's wall,
you might do something like this:

    <mv3d:shape(fence),scale(0.9,1.3),y(0.51),rot(0),z(0)>

---

## Map Configuration

Map configuration goes in the note area of the map settings. Configurations
should be placed in an <mv3d></mv3d> block, which should contain a list of
configuration functions.

Some of these configurations apply when the map is loaded, and some affect how it's rendered

---

	light(color,intensity)
	fog(color,near,far)

Setup the ambient light and fog of the map.

---

	yaw(deg)
	pitch(deg)
	dist(n)
	height(n)
	cameraYaw(deg)
	cameraPitch(deg)
	cameraDist(n)
	cameraHeight(n)

Sets the camera yaw, pitch, distance, and height for the map.   

---

	mode(s)
	cameraMode(s)

Set either PERSPECTIVE or ORTHOGRAPHIC mode.

---

	edge(true/false)

If false, walls aren't rendered at the edges of the map.

---

	ceiling(img,x,y,height)

Uses the tile texture specified by `img,x,y` to render a ceiling for the map.  
Good for indoor areas. If height isn't specified, default will be used.

---

## Plugin Commands

In the following commands, the parts surrounded with angle bracks such as <n> are parameters.

Some commands (like lamp and flashlight) act on a character. By default the
target character will be the current event.
You can define your own target using the following syntax:

	mv3d @target rest of the command

If the second word in the command starts with `@`, that will be interpreted
as the target.   
Valid targets:

- @p or @player: Targets $gamePlayer.
- @e0, @e1, @e2, @e25 etc: Targets event with specified id.
- @f0, @f1, @f2, etc: Targets first, second, third follower, etc.
- @v0, @v1, @v2: Boat, Ship, Airship.

Some parameters can be prefixed with + to be set relative to the current
value.

For example:

	mv3d camera yaw +-45 0.5

---

	mv3d camera pitch <n> <t>
	mv3d camera yaw <n> <t>
	mv3d camera dist <n> <t>
	mv3d camera height <n> <t>

Sets the camera properties, where <n> is the new value and <t> is the time to
interpolate to the new value.   
Prefix <n> with + to modify the current value instead of setting a new
value.

---

	mv3d camera mode <mode>

Set camera mode to PERSPECTIVE or ORTHOGRAPHIC

---

	mv3d rotationMode <mode>

Set the keyboard control mode for rotating the camera.

Modes:

- OFF - Can't rotate camera with keyboard.
- AUTO - A&D while in 1st person mode. Otherwise Q&E.
- Q&E - Rotate camera with Q&E keys.
- A&D - Rotate camera with left & right or A&D. Move with Q&E.

---

	mv3d pitchMode <true/false>

Allow player to control camera pitch with pageup and pagedown.

---

	mv3d fog color <color> <t>
	mv3d fog near <n> <t>
	mv3d fog far <n> <t>
	mv3d fog dist <near> <far> <t>
	mv3d fog <color> <near> <far> <t>

<t> is time.  
Prefix values with + to modify the current values instead of setting new
values.

---

	mv3d light color <color> <t>
	mv3d light intensity <n> <t>
	mv3d light <color> <intensity> <t>

---

	mv3d @t lamp color <color> <t>
	mv3d @t lamp intensity <n> <t>
	mv3d @t lamp dist <n> <t>
	mv3d @t lamp <color> <intensity> <dist> <t>

---

	mv3d @t flashlight color <color> <t>
	mv3d @t flashlight intensity <n> <t>
	mv3d @t flashlight dist <n> <t>
	mv3d @t flashlight angle <deg> <t>
	mv3d @t flashlight pitch <deg> <t>
	mv3d @t flashlight yaw <deg> <t>
	mv3d @t flashlight <color> <intensity> <dist> <angle> <t>

Angle is beam width of the flashlight.

---

	mv3d camera target @t <t>

Change the camera's target.
Camera will transition to the new target over time <t>.

---

	mv3d camera pan <x> <y> <t>
	mv3d pan <x> <y> <t>

Pans the camera view, relative to current target.

---

### Vehicle Commands

	mv3d <vehicle> big <true/false>
	mv3d <vehicle> scale <n>
	mv3d <vehicle> speed <n>
	mv3d airship ascentspeed <n>
	mv3d airship descentspeed <n>
	mv3d airship height <n>

"Big" vehicles can't be piloted too close to walls. This can be useful to
avoid clipping with vehicles with large scales.   
Speed of the vehicle should be 1-6. It works the same as event speed.   
A higher airship can fly over higher mountains. Perhaps you could let the
player upgrade their airship's height and speed.

---