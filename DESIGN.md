# XENOTIME Design Sheet

**Version 1.0.0**

This document is the designs and specifications sheet for the XENOTIME Game Engine. This spec will be updated frequently. Design proposals should update this file before changing any code content! Any sections listed in the index that you can't find in the document are unfinished.

## Markdown Standards/Guidelines

- **Maximum column width** should be **128 characters**
- Lists should **prefer** `-` over `*` or `+`

## Index

- Engine Information and Specifications
- Directory Structure
- Modules
	- External
		- Audio Effects
		- Shader Effects
	- Internal
		- The Core Engine (`xenotime`)
			- Initialization
			- Game Loop
				- Tick
				- Render
		- The Audio Engine (`xsound`)
		- The Post-Processing Effects Engine (`xpostproc`)
		- The UI Framework (`xui`)
		- The Map Loader (`xmap`)
		- The Content Loader (`xcontent`)
		- The Raytracing Engine (`xrace`)
		- The Network/Multiplayer Engine (`xnet`)
- Maps
- Models
- Animation/Faceposing
- Scripting/Config
- The Console
- Non-Module Utilities
	- XENOMAP
	- XENOMATE

## Engine Information and Specifications

**XENOTIME** is a 2D/3D game engine/platform written in pure Rust, using SDL2. Shaders are all written in GLSL. XENOTIME is designed specifically so it can be **highly compatible**, meaning it is cross-platform, integrates with Steam or runs standalone, and has (albeit limited) backwards and forwards compatibility.

## Directory Structure

The distributed package is only a slightly different version of the source tree. The `make package` script simply creates a copy of the repository (minus the `deploy/` directory), removes all source files, organizes the directory structure and archives it into a tarball (or the archive/compression specified with `-a`/`-c` or `--archive`/`--compression`).

The directory tree should look similar to this:

```
xenotime
├── bin
│   ├── deps
│   │   └── clap.rlib
│   ├── mod
│   │   ├── core
│   │   │   ├── xcontent.rlib
│   │   │   ├── xenotime.rlib
│   │   │   ├── xmap.rlib
│   │   │   ├── xpostproc.rlib
│   │   │   ├── xsound.rlib
│   │   │   └── xui.rlib
│   │   └── ext
│   └── xenotime
├── cfg
│   ├── mapload.xscr
│   └── startup.xscr
├── res
└── src
    ├── bin
    │   ├── xenomap.rs
    │   ├── xenomate.rs
    │   └── xenotime.rs
    ├── game_loop
    │   └── mod.rs
    ├── init
    │   └── mod.rs
    ├── lib.rs
    ├── xcontent
    │   └── lib.rs
    ├── xmap
    │   └── lib.rs
    ├── xpostproc
    │   └── lib.rs
    ├── xsound
    │   └── lib.rs
    └── xui
        └── lib.rs
```

## Modules

XENOTIME is a modular engine. Everything in it, including the core, is a module. Each module has a specific purpose, such as audio processing or map loading. Modules are Rust crates, specifically built for XENOTIME.

### External

**External Modules** are called **Addons**. Addons are placed in the `bin/mod/ext/` directory. Their resources are placed in `res/addon/`

### Internal

**Internal Modules** can also be called Engine Modules, Core Modules, or just Engines, as they are smaller core parts that essentially make up the game engine itself as a whole. Most of these are stages in the game loop, except of course the loop itself. Placed in `/bin/mod/core/`

#### The Core Engine (`xenotime`)

**The Core Engine** is the central library that brings all the core modules together.

##### Resources

The XENOTIME Resource Manager centers around an index of all the models, textures, sounds, maps, and other resources that are in a certain game. This index is stored in `res/index/`, and keeps record of all the resources so that they can be easily loaded along with each map.

#### The UI Framework (`xui`)

The **UI Framework** is used to design, manage and control UI, GUI, and HUD.

#### The Map Loader (`xmap`)

The Map Loader `xmap-load` is quick and seamless in design. Map files (`.xmap`, placed in `res/map`) are minimal binary files, so that they can load very quickly.

Don't you hate how maps load so slowly in other engines, and pause the game too? `xmap-load`, Unlike other map loaders, does not operate on the same thread as the game. When a map is loaded, a new thread is spawned to load the next map, and it is merged with the currently loaded map. Once the old map is out of range, it is unloaded. Maps are loaded ahead of time, and there's usually about 2-4 maps loaded at all times. It's up to the game designer to actually use this right, because this design quite heavily relies on good trigger placement.

## Maps

Maps in XENOTIME are `.xmap` files. They are created with XENOMAP and loaded into the engine with `xmap`.

## Animation/Faceposing/Scenes

Scenes/animations in XENOTIME are `.xscene` files. Scenes are created with XENOMATE (`xenomate`). They are applied in real time with Events.

## Scripting/Config

Everything that happens in XENOTIME is based around Events. The XenoScript language (`.xscr`) is a scripting language (not unlike UNIX shell scripts) that simply executes Events line-by-line. Config files (`.xcfg`) are the same except they can only change variables (`let variable_name value`).

## The Console

**The Console** is an interface for executing Events.
