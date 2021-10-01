# sketchrip
Ripper and Conversion for Sketchfab Models
Please note this repository has no support. I literally do not care about this project. This repo will never be updated. Don't bother opening issues or pull requests - I won't answer them.
Feel free, though, to fork the repo and fix/improve it.
Also note that this software is janky, buggy, and doesn't work all the time: expect to do some manual tinkering to fix it.
# How does this work?
Sketchrip consists of two main components.
## 1 - Ripper
The ripper steals the 3D models right out of the embedded model preview window on any Sketchfab page, and downloads them as .obj files, as well as the corresponding .mtl material settings file. In addition, it also opens all the texture image files as new tabs.
## 2 - Converter
The converter, developed by CesiumGS, converts OBJ files, which do not import all the textures correctly into programs like Blender, into glTF files, which do import correctly and package all the textures into one neat file.
# Installation / Usage Instructions
Note: the use of the converter is totally optional. Without conversion, you will still be left with perfectly functional OBJ files, and a bunch of nice texture files. The only reason to use the converter is that Blender (not tested on other software) has a super janky and archaic OBJ importer, and as such will not import all of the texture map files. It will typically only import the base colour, normal map, and emission map, and will not import the roughness or metalness maps, both of which are important for correctly representing PBR materials.
## Requirements:
 - Firefox (not tested on other browsers)
 - Tampermonkey Extension
 ### Recommended:
 - Pull Tabs extension (downloads all the image files quickly so you don't have to right click -> save as for like 20 files)
### If you are using the converter:
 - Node.js
## Installation and Usage

