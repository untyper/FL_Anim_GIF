# FL_Anim_GIF
Original project: https://github.com/wcout/fltk-gif-animation

# Changes
Added functionality to load GIFs from memory buffers.

- Removed redundant x,y constructor in favor of default arguments and reversed the order of name and position parameters for better overload resolution.
+ Added `Fl_Anim_GIF` constructor overload to support loading GIFs from memory buffer.
```c++
  Fl_Anim_GIF(const char *name_, int x_, int y_, int w_ = 0, int h_ = 0,
              bool start_ = true, bool optimize_mem_ = false, int debug_ = 0);
  Fl_Anim_GIF(const char *name_, const unsigned char *buf_, int len_,
              int x_, int y_, int w_ = 0, int h_ = 0,
              bool start_ = true, bool optimize_mem_ = false, int debug_ = 0);
```

+ Added `_init` overloads:
```c++
  void _init(bool start_); // tunneling point for both original _init and new overload
  void _init(const char *name_, char *buf_, int len, bool start_, bool optimize_mem_, int debug_); // new overload
```

+ Added `load` overload:
```c++
bool load(const char *name_, char *buf_, int len_, bool from_file_ = false);
```

+ Added `set_name` helper function for `load`
```c++
void set_name(const char *name_);
```

The changes above are also, obviously, reflected in the implementation file (.cxx)

# Installation
Either place the files directly in your source directory or compile to a stand alone static library and link against it (`fltk_gif.lib`).
Remember to include `Fl_Anim_GIF.H` in both cases.

# Usage

```c++
unsigned char my_gif[] = {/* some gif bytes here */};
Fl_Anim_GIF animgif(0, 0, 500, 500, NULL, my_gif, sizeof(my_gif));

// See examples folder for more
```

# Remarks
- This repo only modifies the `extern` version of the original project.
- Modifications tested on `FLTK 1.3.9`
