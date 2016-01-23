# Xenotime Design Sheet

**Version 0.0.1**

This document is the design sheet for the Xenotime Game Engine.

## Index

- Engine Information and Specifications
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
- Templates

## Engine Information and Specifications

**Xenotime** is a 2D/3D game engine/platform primarily written in Rust, C, and minimal amounts of Assembly. The graphical engines (the 3D Rendering Engine, the Shaders Engine, and the Post-Processing Effects Engine, etc.) use OpenGL. Shaders are all written in GLSL.
Xenotime is designed specifically so it can be **highly compatible**, meaning it is cross-platform, integrates with Steam or runs standalone, and has (albeit limited) backwards and forwards compatibility.

## Initialization



## Game Loop



## Modules

Xenotime is a modular engine. Everything in it, including the core, is a module. Each module has a specific purpose, such as audio processing or map loading. Modules are `.rlib` files.

### External

**External Modules** are called **Addons**. They can be written in Rust, C, Python, or XenoScript. Addons are placed in the `xenotime/bin/mod/ext/` directory

#### Audio Effects



#### Shader Effects



### Internal

**Internal Modules** can also be called Engine Modules, or just Engines, as they are smaller parts that essentially make up the game engine itself as a whole. Most of these are stages in the game loop, except of course the loop itself.

#### The Game Loop



#### The Audio Engine



#### The Post-Processing Effects Engine



#### The UI Framework

The **UI Framework** is a windowing system and GUI toolkit.

#### The Map Loader

The Map Loader is quick and seamless in design. Map files (`.xmap`) are minimal binary files, so that they can load very quickly.

##### How it works

Unlike other map loaders, it does not operate on the same thread. When the next map trigger is activated, a new thread is spawned to load the next map, and it is merged with the currently loaded map. Once the unload map trigger activates, the old map is unloaded.

#### The Model Loader



## Maps



## Models



## Scripting/Configuration



## Templates

**Templates** are basically the configuration state of the engine. Template names should be in `UPPER_SNAKE_CASE`.

## The Console

**The Console** is simply an interface for changing config values and controlling the engine manually.
