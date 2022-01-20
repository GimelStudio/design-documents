# Node properties

## IMAGE
An RGBA Image object which is a container for a numpy array
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

**Can convert between float and int inexplicitly/automatically though, with some data loss obviously.**

## INT
A positive integer value
- ``12``
- Grey
- NumberField 


# STRING
A text value
- ``"hello there!"``
- Blue
- TextCtrl 


# VECTOR2
Two integers/floats
- ``(120, 255)``
- Purple
- Mutiple NumberFields


# VECTOR3
Three integers/floats
- ``(120, 255, 453)``
- Purple
- Mutiple NumberFields
