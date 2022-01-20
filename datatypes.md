# Node properties

A node property represents either a node socket or widgets in the Properties Panel.

## IMAGE
An RGBA Image object which is a container for a numpy array, or a binding to another node
- ``Image()``
- Yellow
- FilePicker


## COLOR
Four integers in a tuple representing an RGBA value
- ``(255, 255, 255, 255)``
- Green
- ColorPicker


## FLOAT
A float value
- ``12.0``
- Grey
- NumberField (or another widget?)

**Can convert between float and int inexplicitly/automatically, with some data loss obviously.**

## INT
A positive integer value
- ``12``
- Grey
- NumberField 


## STRING
A text value
- ``"hello there!"``
- Blue
- TextCtrl 


## VECTOR2
Two integers/floats
- ``(120, 255)``
- Purple
- Mutiple NumberFields


## VECTOR3
Three integers/floats
- ``(120, 255, 453)``
- Purple
- Mutiple NumberFields
