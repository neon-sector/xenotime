# XENOTIME Design Sheet

**Version 0.0.1**

This document is the designs and specifications sheet for the XENOTIME Game Engine. This spec will be updated frequently. Design
proposals should update this file before changing any code content!

## Markdown Standards/Guidelines

- **Maximum column width** should be **128 characters**
- Lists should **prefer `-`** over `*` or `+`

## Index

- Engine Information and Specifications
- Directory Structure
- Initialization
- Loading Resources
- Resource Storage
- Game Loop
- Modules
	- External
		- Audio Effects
		- Shader Effects
	- Internal
		- The Core Engine
		- The Audio Engine
		- The Post-Processing Effects Engine
		- The UI Framework
		- The Map Loader
		- The Model Loader
- Maps
- Models
- Scripting
- The Console

## Engine Information and Specifications

**XENOTIME** is a 2D/3D game engine/platform primarily written in Rust. The graphical engines (the 3D Rendering Engine, the
Shaders Engine, and the Post-Processing Effects Engine, etc.) use OpenGL and Piston. Shaders are all written in GLSL. XENOTIME
is designed specifically so it can be **highly compatible**, meaning it is cross-platform, integrates with Steam or runs
standalone, and has (albeit limited) backwards and forwards compatibility.

## Directory Structure

The distributed package is only a slightly different version of the source tree. The `script/package.sh` script simply creates a
copy of the repository (minus the `deploy/` directory), removes all source files, and archives it into a tarball (or the
archive/compression specified with `-a` or `--archive`). It will throw an error if you haven't built yet.

## Initialization



## Game Loop



## Modules

XENOTIME is a modular engine. Everything in it, including the core, is a module. Each module has a specific purpose, such as
audio processing or map loading. Modules are Rust crates, specifically built for XENOTIME.

### External

**External Modules** are called **Addons**. Addons are placed in the `bin/mod/ext/` directory

#### Audio Effects



#### Shader Effects



### Internal

**Internal Modules** can also be called Engine Modules, or just Engines, as they are smaller parts that essentially make up the
game engine itself as a whole. Most of these are stages in the game loop, except of course the loop itself.

#### The Game Loop



#### The Audio Engine



#### The Post-Processing Effects Engine



#### The UI Framework

The **UI Framework** is a windowing system and GUI toolkit.

#### The Map Loader

The Map Loader (`xmap-load`) is quick and seamless in design. Map files (`.xmap`, placed in `res/map`) are minimal binary files, so that they can load very
quickly.

##### How it works

Don't you hate how maps load so slowly in other engines, and pause the game too? `xmap-load`, Unlike other map loaders, does not
operate on the same thread as the game. When a map is loaded, a new thread is spawned to load the next map, and it is merged
with the currently loaded map. Once the old map is out of range, it is unloaded. Maps are loaded ahead of time, and there's
usually about 2-4 maps loaded at all times. It's up to the game designer to actually use this right, because this design quite
heavily relies on good trigger placement.

#### The Model Loader



## Maps

Maps in XENOTIME are `.xmap` files. They are created with `xenomap` and loaded into the engine with `xmap-load`.

## Models



## Scripting/Configuration



## The Console

**The Console** is simply an interface for changing config values and controlling the engine manually.
