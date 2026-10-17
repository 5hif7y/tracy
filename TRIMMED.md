# Tracy recortado: solo Vulkan y Metal

Esta rama parte de Tracy 0.14.2 (commit `778d6d71`) y conserva únicamente
los backends de GPU que usa Mirai:

- **Vulkan** (`public/tracy/TracyVulkan.hpp`).
- **Metal** (`public/tracy/TracyMetal.hmm`).

## Qué se quitó

Del cliente y del árbol de ejemplos/pruebas:

- Direct3D 11 y 12 (`TracyD3D11.hpp`, `TracyD3D12.hpp`).
- OpenGL (`TracyOpenGL.hpp`).
- OpenCL (`TracyOpenCL.hpp`).
- CUDA (`TracyCUDA.hpp` y `tests/cuda/`).
- WebGPU (`TracyWebGPU.hpp`).
- ROCm/ROCProf (`tests/rocprof/` y el enlace opcional con
  `rocprofiler-sdk` en `CMakeLists.txt`).

También se quitaron los ejemplos que dependían de esos backends (CUDA,
OpenGL, WebGPU, OpenCL y el ToyPathTracer de D3D11) y sus listas en
`CMakeLists.txt` y `meson.build`.

El **profiler GUI no se recorta**: sigue siendo el de Tracy 0.14.2 y puede
mostrar cualquier tipo de contexto que reciba. Lo que cambia es el cliente:
solo emite zonas de Vulkan y Metal.

El manual (`manual/tracy.tex`) y las notas de achievements conservan texto de
los backends upstream; son documentación de referencia, no soporte incluido
en esta rama.

## Build

Windows x64, MSVC y CMake:

```bat
cmake -S . -B build -G "Visual Studio 18 2026" -A x64 ^
  -DBUILD_SHARED_LIBS=ON -DTRACY_ENABLE=ON
cmake --build build --config Release --target TracyClient
```

El resultado es `TracyClient.dll` y `TracyClient.lib`. El GUI se compila
aparte, desde `profiler/`:

```bat
cmake -S profiler -B build-profiler -G "Visual Studio 18 2026" -A x64
cmake --build build-profiler --config Release
```

## Licencia y redistribución

Tracy se distribuye con la licencia BSD de 3 cláusulas; el aviso completo está
en `LICENSE` y en las cabeceras de los archivos. Los binarios de este fork se
construyen desde este árbol y conservan esos avisos. Las dependencias del GUI y
del servidor (capstone, zstd, GLFW, FreeType, pugixml, libcurl, etc.) tienen sus
propias licencias; al redistribuir los binarios hay que incluir sus avisos.
