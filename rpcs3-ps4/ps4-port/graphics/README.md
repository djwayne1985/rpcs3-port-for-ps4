# Graphical presentation on PS4

## Vulkan

The `rpcs3_ps4_vulkan` target compiles all Vulkan files listed by RPCS3's CMake,
that is, the original `VKGSRender`. The frontend compiles with
`HAVE_VULKAN=1`. `ps4-vkloader` (from orbis-porting-kit) provides the symbols
Vulkan static files and links the RADV driver and zlib from the Mesa PS4 package. The
Vulkan headers come from the same package.

On PS4 the surface is created with `VK_EXT_headless_surface`; Mesa PS4
presenta the headless swapchain through `sceVideoOut`. The
[triangle example from orbis-porting-kit](https://github.com/orbis-ports/orbis-porting-kit/tree/main/examples/triangle)
shows this presentation path in isolation.

RADV is linked as a whole (`WHOLE_ARCHIVE`), because Mesa's dispatch tables
reference many handlers only weakly.
`verify-native-dispatch.py` checks the final ELF for mandatory handlers
exist and that the dispatch table points to them.

## OpenGL

The original `GLGSRender` uses Mesa EGL with Zink over the same RADV. Mesa
only replaces the platform context/window and function loader.
`prepare-mesa-gl.py` removes identical objects from EGL and Gallium files
to those that RADV already contains, keeping duplicate symbol errors to
any other conflict.
