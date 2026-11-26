# Importing 3D models with GLTF Scene

Learn how to load a GLB model into cables, inspect its structure, hide parts of the scene, and replace its materials. Then use Blender to convert a downloaded glTF model into a GLB for use in cables.

Based on [GLTF tutorial 01 by Mark aka andro](https://www.youtube.com/watch?v=QNLw21hSweg), dated February 10, 2020. Operator labels and interface details follow the tutorial and may differ in newer versions.

## What you need

- A cables patch with MainLoop, OrbitControls, Transform, and a Lambert material already set up.
- A GLB model to import.
- Blender and a downloaded glTF model for the conversion exercise.

## Understand glTF and GLB

 glTF is a format for exchanging 3D scenes. GLB is its binary variant. In this tutorial, GLTF Scene is the operator used to load the model, and the uploaded file has a `.glb` extension.

## Load a model

1. Add a **GLTF Scene** operator to the existing rendering setup. Search for `GLTF` in the operator picker.
2. Drag your `.glb` file into cables and wait for the upload to finish.
3. Select GLTF Scene, click **Browse**, and choose the uploaded file.
4. Use the orbit controls to inspect the model.

The first example uses the Sponza scene, a model commonly used for rendering tests.

### Adjust position and scale

GLTF Scene has an **Auto Scale** option enabled by default. It uses the scene bounds to help fit the imported model into view, but it does not guarantee that every model will appear correctly.

If the model is missing or the view appears to be inside it:

1. Try toggling Auto Scale.
2. Disable Auto Scale if you want to control the size yourself.
3. Adjust scale and position with Transform.
4. Check for position offsets left over from a previous model, especially the Y position.

## Inspect and hide scene parts

Select GLTF Scene and click **Show Structure**. The structure panel lists the scene's materials and nodes. Nodes can represent mesh parts or objects such as empties.

Use the eye icons to hide or show parts of the scene. These visibility settings belong to the operator: saving the patch and reloading it preserves them. They are more than a temporary preview.

## Replace a material

1. Open **Show Structure** and identify the mesh part you want to change.
2. Find the material associated with that part in the material list.
3. Click **Assign** for that material. cables creates a material assignment operator, described in the video as **GLTF Set Material**.
4. Add a Lambert material and connect its shader output to the assignment operator.
5. Supply the required trigger connection to the material setup.
6. Change the Lambert material's color and check the result on the model.

Make sure the target mesh part is visible. A hidden part will not show the color change.

In the Sponza example, the tutorial replaces the materials for a fabric section and an arch. Repeat the same process for other material entries. Lambert is used to make the color changes easy to see; the tutorial also mentions Basic and Phong materials as alternatives.

The assignment follows the selected material entry. Check the structure panel to see which scene parts use it rather than assuming each visible object has a unique material.

## Convert a downloaded model in Blender

The second example uses a low-poly tree scene downloaded from Sketchfab. Choose a model you have permission to use and retain its required credits.

1. Download the model in glTF format.
2. Open Blender and clear the default scene by selecting its objects and deleting them.
3. Use **File → Import → glTF** to load the downloaded scene.
4. Inspect the imported model and select the objects for export.
5. Open Blender's glTF export dialog and choose **GLB** (`.glb`) as the output format.
6. Keep materials included in the export. The material replacement workflow in cables relies on the model's material entries.
7. Choose a filename and export the scene.
8. Drag the exported GLB into cables, then choose it with GLTF Scene's Browse control.
9. Adjust Auto Scale, Transform scale, and position until the model is visible at a useful size.

The tutorial uses the default export settings with materials included. Conversion can still involve problems with other models; this example deliberately uses a simple, well-prepared asset.

## Give the tree scene separate colors

Open Show Structure for the imported tree scene. The example has four material entries.

Create an assignment operator for each material, connect a separate Lambert material to each one, and supply the trigger connections. Change the colors one at a time to identify the parts they affect:

- Bark: brown.
- Foliage: green.
- Ground: a darker green.
- Underside or base: a separate color as needed.

If you cannot tell what an assignment affects, return to the structure panel and inspect its material entry and associated mesh parts.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| No model appears | Confirm that the upload finished and the file is selected. Try Auto Scale, then inspect Transform scale and position. |
| The camera seems to be inside the model | Reduce the model scale or try Auto Scale. |
| A color change has no visible effect | Check the target part's visibility, the selected material entry, the shader connection, and the trigger connection. |
| Parts remain hidden after reloading | Restore their visibility in Show Structure; visibility settings are saved with the operator. |
| The converted model lacks usable material entries | Check that materials were included in the Blender export. |

## Operator descriptions for reuse

### GLTF Scene

Loads and displays a 3D scene from a GLB file in this workflow. Auto Scale helps fit the imported scene into view. Show Structure exposes the scene's materials and nodes so you can inspect them, save visibility choices, and create material assignments.

### GLTF Set Material

Assigns a cables material to a selected material entry in an imported GLTF scene. Create the assignment through the Assign button in GLTF Scene's structure panel, then connect a material's shader output and the required trigger setup. Use it to recolor or restyle the scene parts associated with that entry.

### Transform

Controls the imported model's placement and scale in this setup. Use it to correct position offsets or adjust model size manually when Auto Scale does not produce the desired view.

### Lambert material

Provides the replacement surface material used in the examples. Separate Lambert materials let you adjust the colors of the imported scene's material groups independently.
