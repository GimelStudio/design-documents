Gimel Studio Technical Design Document
======================================

Draft of the Gimel Studio application design document


Goals
-----

The technical goal is to allow visual scripting of image effects, transformations, procedual generated images, etc, via nodes.

The goal is to allow the user to create his/her own library of "presets" (node setups) to share and re-use in a way that traditional image-editing program systems often limit.

This could be useful for batch processing and anything from a quick resize to an advanced color-correction setup.

An API for scripting the nodes themselves in Python for those who want to go deeper than the nodes already provided, should be included.

The Gimel Studio core render engine needs to be fully detatched from the UI so that it can be used headless (batch processing from CLI, etc)

Design Decisions
----------------

1. The goals and technical standpoints of Gimel Studio don't really align well with including krita-like-painting-functionality.
 - Thus, focus will not be placed on creating high-quality painting tools except for paint tools for creating masks, etc.

2. Python and GLSL will be the API languages (GLSL will be optional).

3. Cross-platform (Windows, Linux, MacOs), support for 64-bit systems (32-bit not planned and may not be possible from a technical standpoint).


Application Details
-------------------

1. It starts with a JSON file defining everything no matter where the file came from.

 * The JSON file defines all the Node Graphs and node connections within them

 - From CLI (Commandline) or From file path
  - Error checking/version compat checking
 - From the Blender addon
  - Works similar to the From CLI except that JSON file will probably load differently
 - New file or no file specified = "default", blank JSON file
  - Pre-defined default file
 - From "Template" such as PBR texture generation, etc
  - Loads a template JSON file


2. The JSON file is parsed and turned into Python objects -> Node Graph UIs, etc.

 * A Node Graph is defined as a "Layer"
 * There is 1 Node Graph "Layer" that is the "master" layer which composites the other Node Graph "Layers" together.
 * Each Node Graph Layer needs its own id and should only be re-rendered if a change was made in that Node Graph.


3. A large editing view (common to traditional photo-editors) is the main panel.

 * Adding effects/transforms


4. The layer panel with Node Graph Layers allows for a quick way to see the stacked layers

 * The "Layers" view (which is the "Master" node graph) shows a simplified node graph with parameters exposed??
  - How should parameters be exposed in the layers??


5. The render engine cycles through the nodes rendering their results, with a "smart cache" system to only re-render (evaluate) nodes that are further down the node tree.

 - A. Evaluation starts at the Master Node Graph
   - For each layer, the renderer checks to see if the layer was changed. If the layer was edited, the renderer continues to that Node Graph otherwise, it uses the cached image result of the layer and continues to the next layer.
 - B. When a layer Node Graph needs to be rendered
   - The evaluations starts at the Output Layer node. As it cycles through the node tree, it will evaluate until it finds the node that is marked "edited" (this is the node that was edited by the user). The renderer gets the cached result of the node before it and uses it to render the Layer.