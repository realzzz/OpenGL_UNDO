OpenGL_UNDO
===========

Support UNDO in openGL 


Note - the code is still being used by commercial tools so it can not be published here. I'm just puting the ideas of how to do it.


JERRY OPENGL ES UNDO IMPLEMENTATION
 
 OPENGL (for opengl es 1.1) only allows you to draw things into the buffer while it does not support the UNDO function. So there is no perfect native support. 
 
 Here - drawing of paint brush needs to be able to get function of undo. While obvioulsy we can come up with such idea:
 1. record all the drawing step's information.
 2. ignore last one when undo happens. (not delete - remember redo)
 3. clear the screen and re-draw everything. 
 
 but this is ofcourse heavy operation - when people draws over 20 lines (very likely case), it could take over 5 seconds or more to do so. 
 
 So my current solution is - instead of drawing everything everytime, let's copy the buffer for certain number of step's distance:
 
 1. draw and record.
 2. if it has already drawed N step without copy buffer, then save the buffer to texture.
 3. when user do undo, find the nearest buffer texture, store it back to screen, then draw the left over strokes (which is less than N for sure). 
 
 This is a trade of memory space and efficiency, however, due to the fact that texture's size needs to be power of 2, every copy of buffer needs to add 1024*1024*4 size. so - image you have like 10 buffer stored - 40M space! ;-( No idea how to improve... 
 
 Another note is.. stored image from glpixel to texture is upside down. needs to glroate and gltranslate after putting into view.
