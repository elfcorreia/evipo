# Viewport Library

This library was designed for educational purposes in such a way that:

* Enables C/C++ programming students works with graphics as early as possible
* Graphical approach through the framebuffer abstraction
* Provides multiple implementations to embrace any student's software resources
* Easier compilation and linkage

## Quick start
    
    viewport* v = viewport_create(320, 240, "");
    int buffer[320][240];
    buffer[100][50] = 0x0f0ff0;
    
    while (!viewport_finished(v)) {
      viewport_sync(v, buffer);
    }
    viewport_destroy(v);

## Operations

- `viewport* viewport_create(int viewport_width, int viewport_height, const char* options);`
  
  Createas and initialize a viewport.

- `void viewport_sync(viewport* instance, void* framebuffer);`
  
  Writes the buffer pointed by `framebuffer` into viewport instance.

- `bool viewport_finished(viewport* instance);`

  Checks if there is a finish request that can be a close button in a window of a explicit call of `viewport_finish()`

- `void viewport_finish();`
  
  Finalize the viewport.

- `void viewport_destroy(viewport* instance);`

  Destroy the viewport

## Details

### Framebuffer abstraction

The framebuffer abstraction consist of a piece of memory (buffer) to store pixels that will be render on a graphical device. This graphical device is platform dependent. So can be an X11 window, a Win32 window or an OpenGL context.

### Pixels and colors

Each pixel color is represented by an RGB value in an `int`. Is easy to think in colors using hexadecimal literals, like HTML colors:

    int red = 0xff0000;
    int green = 0x00ff00;
    int blue = 0x0000ff;

### Viewport size and buffer size

A Viewport is an area where the buffer will be rendered. In many situations, to render an buffer of 320 by 240 in a viewport of 320 by 240 is trivial. However, you can too render an buffer of 9x9 pixels in a viewport of 100x100. In such situations the viewport makes an upsampling. 

Downsampling is not available.

### Framebuffer memory layout

The argument `framebuffer` can be:

- a pointer to a unidimensional array: `int framebuffer[32*32];`
- a pointer to a bidimensional array: `int framebuffer[32][32];`
- a pointer to a heap allocated memory: `int* framebuffer = new int[32*32];`
- etc...      

It must be always: `framebuffer[lines][columns]`

### Erro handling

On errors a panic function is called. It's prints a message and terminates the program with a call for `exit(1)`.