Gimel Studio UI Design Document
===============================

*DRAFT - open for discussion. :)*


The UI is WIP. See mockups and code in the main repository. :)


## Menubar

- File
  - New Project
    - Blank (Default)
    - From Template
  - Open Project
  - Open Recent Project
  - Save Project
  - Save Project As
  - Save As Template
  - Quit

- Edit
  - Copy Image to Clipboard
  - Preferences

- Render
  - Render Image
  - Auto Render Image

- View
  - Toggle Window Fullscreen
  - Maximize Window
  - Show Statusbar

- Help
  - Online Manual
  - Visit Website
  - Report a Bug
  - About Gimel Studio


## Panels

Panels are re-arrangable, resizable sections of the interface where the various Editor Areas are placed.

### Panel Topbar

Each panel can have a Panel Topbar which has various widgets to change settings and view info about the Editor Area.

Panel topbar menu (three dots icon button) at far right shows:
- Undock panel (float the panel so it can be re-arranged)
- Hide panel (hide the panel, can be re-shown using the View > Toggle the_panel menu or a shortcut)
- Open panel as new window (opens the panel as a new window, useful for multi-monitor setups)??!


## Editor Areas

An editor is the actual area where the main interaction happens.

### Image Viewport

The Image Viewport is an area for viewing the image and interacting with the node properties in a visual way -via Gizmos.

- Tab/Radio buttons to switch between: 
   - Context (Shows the result of the currently selected node)
   - Viewer (Shows the result of the viewer node)
   - Final / Output (shows the result of the final output node)

- Tab/Radio buttons to switch between: 
   - Checkerboard background
   - Plain background (just the editor background color)
   - Grid (grid is defined elsewhere?)

- Control for setting viewport zoom

- Toggle showing before/after view, which shows the start image and output node image side-by-side (maybe this is a viewport overlay)??

- Toolbar for Toolsets, Tools and Gizmos (show/hide)??

- Gimzos in Viewport needs Python API

### Node Graph

The Node Graph is the area where users interact with the nodes themselves. 

### Node Properties

The Node Properties is the area where properties of the selected node are edited.


## Gizmos & Toolsets

A Gizmo is a viewport widget for interacting with the Node Properties in a visual way. When a node has Gimzo(s), a small toolbar (Toolset) will appear on the left side of the Image Viewport allowing for quick changing between Gizmos.

Each Node can have multiple Toolsets to edit the Node Property values, and is node dependent. The Toolset will change based on which node is selected. The Toolset will create Gizmo for user to manipulate. User can use either the Gizmo or the Properties Editor Area to Edit the Node Properties.

Gizmos & Toolsets are defined in the Python API by the node developer.

- Each Node will have a Toolset
- Toolset can have one or more tool
- Each tool will have its own Gizmo to Manipulate the Properties

Example Nodes:

- **Crop Node**

  - [Toolset]
    - [Tool] Crop
      - [Gizmo] Crop the image

Rather than editing the four values of a Crop node, a Gizmo (a rectangle with moveable handles in this case) is used to define the property values in a visual way.

- **Transform Node**

  - [Toolset]
    - [Tool] Translate
      - [Gizmo] Translate image

    - [Tool] Rotate
      - [Gizmo] Rotate image

    - [Tool] Scale
      - [Gizmo] Scale image

- **SVG Input Node (Future Possibility)**

  - [Toolset]
    - [Tool] Edit Points
      - [Gizmo] Points Become Editable and Able to Move the Point
      - [Gizmo] Can Press delete to delete selected points

    - [Tool] Add Points
      - [Gizmo] Change the cursor to Add Point Cursor

    - [Tool] Remove Points
      - [Gizmo] Click Point to remove Point

    - [Tool] Angle Points
      - [Gizmo] Turn the Point into 90 degree angle

- **Image Input Node (Future Possibility)**

  - Toolset
    - Paint Brush
    - Eraser
    - Pen Tool (Raster)
    - Shape Tool

- **Mask Node (Future Possibility)**

  - Toolset
    - Paint Brush
    - Eraser
    - Pen Tool (Raster)
    - Shape Tool