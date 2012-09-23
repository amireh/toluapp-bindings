A collection of [tolua++](http://www.codenix.com/~tolua/) packages that can be
used to generate C++ application bindings for use in Lua.

## Usage

Clone the repository, get [CMake 2.8](http://www.cmake.org/), Lua 5.1, and tolua++, then create a `build` directory inside
the repository root. Run `ccmake ..` and choose which packages you want to build. Generate the make config and run `make`.

The libraries will be built in `/path/to/repo/lib` and they will be prefixed by `lua`, so when building the OIS package
the output library will be called `luaOIS.so`. To use it in Lua, you require it like any other script or library,
you just need to make sure Lua can find it by its `package.cpath`:

```lua
package.cpath = "/usr/local/lib/?.so;" .. package.cpath -- or wherever you're storing the libs

require 'luaOIS' -- the module is accessible without the 'lua' prefix for convenience:

print(OIS.KC_RETURN)
```

Note about renaming the binding libraries: you shouldn't rename the library file because Lua expects the init function
to follow the required file's name (ie, `luaopen_luaOIS`), so by renaming the file to `OIS.so`, requiring that
library will no longer work. If you really want to change the names of the libraries, check out the CMake
macro called `GENERATE_BINDINGS`; it's where the prefix is assigned.

Again, if you've missed it in the usage example: the module will be finally accessible **without the `lua` prefix**.
So when binding `luaOIS`, you can access the module using simply `OIS`.

## Packaged applications

### Ogre 1.8

**Wrapper completion status: miserably incomplete**

Wrapped classes:

  1. SceneManager
  1. Nodes
  1. SceneNodes
  1. MovableObject
  1. Entity
  1. Light
  1. Camera
  1. RenderTarget
  1. RenderTarget::FrameStats
  1. RenderWindow
  1. Viewport
  1. MeshManager (plane construction only)
  1. ResourceGroupManager
  1. ResourceManager
  1. CompositeManager
  1. Billboard
  1. BillboardSet
  1. Vector2
  1. Vector3
  1. Degree
  1. Radian
  1. Angle
  1. Quaternion
  1. ColourValue
  1. Plane (construction only)
  1. AxisAlignedBox
  1. OgreBites::SdkCameraMan

Wrapped ENUMs:

  1. PixelFormat
  1. PixelFormatFlags
  1. PixelComponentType
  1. FogMode
  1. CullingMode
  1. ManualCullingMode
  1. ShadowTechnique
  1. PolygonMode
  1. RenderQueueGroupID
  1. AxisAlignedBox::Extent
  1. AxisAlignedBox::CornerEnum
  1. Node::TransformSpace
  1. Light::LightTypes
  1. SceneManager::PrefabType
  1. OgreBites::CameraStyle
  1. BillboardOrigin
  1. BillboardRotationType
  1. BillboardType

### OIS

**Wrapper completion status: fair**

Wrapped classes:

  1. KeyEvent
  2. Keyboard

Wrapped ENUMs:

  1. KeyCode
  2. Keyboard::Modifier

## Shortcomings

**Multiple Inheritance**

While tolua++ does support multiple-inheritance, it's not very transparent. To get around it,
you might see that some packaged classes are literally inlining functionality inherited from
ancestor classes, and in run-time that's really fine. The child class will have access to its
inherited functionality just as it does its own, the only downside is that both the parent
and child packages need to be maintained to reflect the same interface when one is updated.

## License

```
All "binary" packages produced by tolua++-bindings are licensed under the MIT terms.
Kindly note that this license does NOT cover, nor make obsolete, the "source" package(s)
license(s).

Copyright (c) 2012 Ahmad Amireh

Permission is hereby granted, free of charge, to any person obtaining a copy of this
software and associated documentation files (the "Software"), to deal in the Software
without restriction, including without limitation the rights to use, copy, modify, merge,
publish, distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies
or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE
OR OTHER DEALINGS IN THE SOFTWARE.
```