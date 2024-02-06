# FL_Anim_GIF
Original project: https://github.com/wcout/fltk-gif-animation

# Changes
- Added functionality to load GIFs from memory buffers.

Added `Fl_Anim_GIF` constructor overloads to match the original constructors:
```c++
  Fl_Anim_GIF(int x_, int y_, int w_, int h_, char *buf_ = 0, int len_ = 0,
              bool start_ = true, bool optimize_mem_ = false, int debug_ = 0);
  Fl_Anim_GIF(int x_, int y_, char *buf_, int len_ = 0,
              bool start_ = true, bool optimize_mem_ = false, int debug_ = 0);
```

Added `_init` overloads:
```c++
  void _init(bool start_); // tunneling point for both original _init and new overload
  void _init(char *buf_, int len, bool start_, bool optimize_mem_, int debug_); // new overload
```

Added `load` overload:
```c++
bool load(char *buf_, int len_, bool from_file_ = false);
```

The changes above are also, obviously, reflected in the implementation file (.CXX)

# Usage

Either place files in source directory and include or compile to a stand alone static library and link against it (`fltk_gif.lib`).

```c++
char my_gif[] = {/* some bytes here */};
Fl_Anim_GIF animgif(0, 0, 500, 500, my_gif, sizeof(my_gif));

// See examples folder for more
```

# Caveats
To load from a file, the string file name must always be pointed to by a `const char*` for correct overload resolution.
Likewise the GIF memory buffer must NOT be defined with `const` keyword so that the buffer correctly decays to `char*` when passed to `Fl_Anim_GIF`. This will then correctly choose our new memory buffer overload as intended.

i.e.
```c++
const char* my_gif[] = {/*...*/}; // BAD. WRONG OVERLOAD CALLED. APP MIGHT CRASH.
char* my_gif[] = {/*...*/}; // CORRECT OVERLOAD CALLED.
```

# Remarks
- This repo only modifies the `extern` version of the original project.
- Modifications tested on `FLTK 1.3.9`
