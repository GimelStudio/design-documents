Gimel Studio Goals & Non-goals
==============================

*Please note that this is still a draft and is open for discussion. :)*


## **Goals**

The technical goal is to allow visual scripting of image effects, transformations, procedual generated images, etc, via nodes.

The goal is to allow the user to create his/her own library of "presets" (node setups) to share and re-use in a way that traditional image-editing program systems often limit.

This could be useful for batch processing and anything from a quick resize to an advanced color-correction setup.

An API for scripting the nodes themselves in Python for those who want to go deeper than the nodes already provided, should be included.

The Gimel Studio core render engine needs to be fully detatched from the UI so that it can be used headless (batch processing from CLI, etc)


## **Design Decisions**

1. The goals and technical standpoints of Gimel Studio don't really align well with including krita-like-painting-functionality.
 - Thus, focus will not be placed on creating high-quality painting tools except for paint tools for creating masks, etc.

2. Python and GLSL will be the API languages (GLSL will be optional).

3. Cross-platform (Windows, Linux, MacOs), support for 64-bit systems (32-bit not planned and may not be possible from a technical standpoint).