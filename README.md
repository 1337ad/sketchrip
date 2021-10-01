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
The converter, developed by [CesiumGS](https://github.com/CesiumGS/obj2gltf), converts OBJ files, which do not import all the textures correctly into programs like Blender, into glTF files, which do import correctly and package all the textures into one neat file.
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
First, install the Tampermonkey extension on Firefox.

Open the extension, and click "Create New Script..."

Switch back to this page, and click [here](https://raw.githubusercontent.com/1337ad/sketchrip/main/script.js). Copy and paste that data into the new script box in Tampermonkey, overwriting all the stuff that's already there.

Now, navigate to a sketchfab model. It is recommended to do this in a new Firefox window, as the action of downloading will open a bunch of new tabs for the texture files, cluttering your window.

Once the model has fully loaded, a red "DOWNLOAD" button should appear in the top right of the 3D viewer. Click it.
At this point, you better hope that you disabled "Always ask to save files" in Firefox settings, because otherwise you're going to have to click "save" for all these nice .obj and .mtl files.

At this point, your download folder should have a few .obj and corresponding .mtl files, and you should have a bunch of tabs with image textures in them. 
If you installed the "Pull Tabs" extension like I recommended above, then this should be easy. Open the pull tabs extension, and click "options". Scroll down to "Services" and disable everything except "Download". Scroll to the bottom of the page and hit "Submit", and close the settings page.

Now, close the Sketchfab model page so all you have open is the image files. Click again on the Pull Tabs extension, and click "Download". This should download all the image texture files to you download directory.

Now you have a download directory nice and full of .obj, .mtl, .jpeg and maybe even some .png files. Fantastic. If you don't want to convert the files, your adventure stops here. Congrats!

### Conversion
Place all the files (objs, mtls and images) into a new folder. 

Open up a new cmd window (or Linux terminal or whatever) and run 
`npm install -g obj2gltf`

If you are on Windows, read the next step. If on Linux or MacOS, I don't have a script for you, but you could write one: just loop through every .obj file in the folder, and execute the command `obj2gltf --metallicRoughness -i "input filename here"` for each file.

Windows Only: Click [here](https://raw.githubusercontent.com/1337ad/sketchrip/main/convert.bat), right click, and save as. Save as type "All files", and remove the .txt extension so it saves as a .bat file.

Copy and paste that .bat file into your new folder with all the files. Double click to run it.

Now, you should have a .gltf file for each of your .obj files. You can now delete all the files (including the image files, because they are packed into the .gltf files, so you don't need the originals any more) except for the .gltf files.

And you're done! Congrats! Import into Blender or other application, and profit.

### Final Note
If you run the conversion script, you may note that it throws an error about "composer layer" having no valid geometry. That's fine - I've got no idea what the "composer layer" file is, but it indeed seems to contain no geometry, so it's safe to ignore and delete.
## Troubleshooting
|Issue|Fix|
|--|--|
| "Could not locate .mtl file - using default material" error when running the batch script| I had this issue with a few files, where opening the .obj file in a text editor revealed that it was looking for the .mtl file with a different name. In my case, the mtl file it was searching for had a colon in the path, which Windows removed when it downloaded the .mtl file. To fix, open the .obj file in a text editor, and on the first line, enter the correct name of the corresponding .mtl file. |
| All objects have the same texture when importing into 3d software, when the textures should be different - where the objects are named something like "Object.obj" "Object(1).obj" "Object(2).obj" and so on| Windows automatically renames the .obj and .mtl files by appending a number to the end of them when they are called the same thing. However, the .obj files themselves are all looking for a file named "Object.mtl" not "Object(2).mtl", so all of the .obj files are looking for the same .mtl file. This means all the textures are the same. To fix, open the .obj files in a text editor, and, as above, correct the first line to refer to the correct corresponding .mtl file.|
