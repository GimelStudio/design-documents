# Gimel Studio Technical Design Document

*DRAFT - open for discussion. :)*

**Some of the below info is now OUTDATED**

## **Global Document**

The global document represents an arbitrary "project" (i.e: the output image) with width and height and the settings to go along with it. This is similar to how Inkscape's "document" is handled.

It defines the:
* document size (e.g: A4 297x210)
* output resolution (%)
* file-type for output
* global-settings (document background, etc) 

This allows for vector layers since the size is fixed and not based on a raster image input.

## **Composite Stack / Layer Stack**

#### **Layer Composition**

- It acts like a Root folder, that stores a bunch of effects.
- It will process whatever effect that is inside the composition.
- The Results of a Composition will be Composited in a Top-down manner just like Layers in Photoshop.

It can have some basic properties, where it treats the result of the composite like a image, and applies basic effects.

- Suggested properties would be:
  - Crops
  - Basic Color Adjustment
  - Transformation
  - Drop Shadow
  - Mask

#### **Layer Types**

* Raster layer
* Vector layer

The document itself can be raster or vector but nodes to convert vector to raster will allow it one-way or the other. In the non-node workflow, there is an option to convert the "vector layer" to raster and vice-versa.

#### **Add Layer Composition**

Few Different Ways to add a New Layer Composition (Same thing but different starting Effect)

- **Add Image Layer** (This is used when drag and drop an image to the Composition Stack)

- **Add Text Layer**

#### Node Composition

- A different type of composition for Pure Node User, it is basically a Container with only one specific Effect / Node Tree called "Node Tree"
- Is Meant only for Pure Node Users that Prefers to not stack a bunch of effects

It does not have any Basic Property, However, It have "Exposed Property", where user can set up and determine which property to expose for edits

## **Effects**

- There should be multiple types of Effects
- It is applied in a Layer Composition
- Multiple Effects can be placed inside a composition

A few Important Nodes Are needed in order to Function Properly:

- Effect Stack Input Node
  - To Take the Baton from the previous effect
- Output Node
  - To Pass the Baton to the next effect



#### Types of Effects

**Primitive Effects (Locked)**

- Are Effect that only have one nodes
- The Most Basic Effects
- Example: Blur, Crop, Brightness and Contrast

**Premade Effects (Locked)**

- Are Commonly used effect that are complex, but comes build in to the software
- Example: Vignette, HD Bar, LUT

**Addon / Plugins Effects (Locked)**

- Downloaded or Made by other user, effect that are "Installed"
- The effect is Locked so it feels more "Official"

**Custom Effects**

- Can be exported into Addon / Plugin Effects to share to other people
- User Made Node Tree / Effects
- Can be created Blank, or Using other Effect as template

#### Category of Effects

**Independent Effects**

- Effect that does rely on the Effect Stack Input Node At all
- It must output something for dependent effects to work with

**Dependent Effects**

- Need to have at least one Independent Effects before apply any dependent effects
- Effect that must use Effect Stack Input Node
- Might or might not have input other than Effect Stack Input Node

### Important Effects

- **Image**
  - Independent Effect
  - Primitive Effect
  - Added to new composite as the first effect by default
  - It takes Image Node as input and Output it for dependents effects to use

### **Optional Effects**

- **Text Effect**
  - Independent Effect
  - Primitive Effect
  - Text as input
  - Font type as input
  - Text Properties as input (Size, spacing etc etc)

#### **Effects Properties**

**Properties**

-Name / Label

-Description / Notes

-Mask Input

**Exposed Properties**

The Maker of the effect can decide to expose which properties in the effect to the user to edit. 

## **Mask Object**

Mask is an Object type that can be created, to be accepted by Mask Property inside Composition, Effects, and Node

Properties of a Mask Object

- Name

- Input 
  - Images
  - Vectors
  - Other Composition
  - Independent Effects

## **Mask Input**

**Mask Input is a Properties inside**

1. Composition

2. Effects

3. Mask Input Node



It can Accept

-Mask Object

-Image

-Other Composition



## **Important Nodes**

**Effect Stack Input Node**

It takes the output of the previous effect in the Effect Queue from the output node

**Output Node**

If it is not the end of the Effect Queue, it will output to the next Effect Stack Input node, if it is the end of the layer, it will be outputted into the composite

**Script Nodes**

A Node that accepts a script to be use as effect



#### **Optional Nodes**

(Only Accessible in Node Composition)

**Composition Input Node**

Takes the output of a specific Composition

**Effects Node**

Using Composition Input's output as an input, It will List out the Effects and takes the output of that effects

## **Node Creation**

There are two ways to create **new** node:

1. Scripted Node
2. Node Tree

The scripted node is a node that is created via the API in Python (code). It can access internals that the primitive nodes use, etc

The node tree is a graphical, node-based programming to create effects/"presets" (a.k.a: similar to a Blender node-group) with the primitive nodes and users can define properties to be exposed.

## **Workflow Use Sample**

Example use-sample with this workflow.

*No-node (layers) version:*
1. User opens the application and creates a new document with a size of 1920x1080 (raster or vector layer could be selected at this point, but raster will be the default)
    - A new composition layer is added to the Layer Stack
    - The layer is shown as a transparent image
2. The user decides to BLUR the image, so they click the menu to add a new effect. They select blur and it is added to the stack underneath the layer as an effect. 
3. In the properties panel, the properties of the blur can be adjusted.

*Node version:*
1. User opens the application and creates a new document with a size of 1920x1080 (raster or vector layer could be selected at this point, but raster will be the default)
    - A new composition layer is added to the Layer Stack
    - The layer is shown as a transparent image
2. The user decides to BLUR the image, **but they want to do it with nodes**, so they switch to the editing tab and add a Blur node. 
3. In the properties panel, the properties of the blur can be adjusted.


## Other technical ideas yet to work out

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