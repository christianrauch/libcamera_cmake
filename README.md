**This CMake wrapper is now deprecated. If you want to build `libcamera` as part of a colcon workspace, then install [`colcon-meson`](https://github.com/colcon/colcon-meson) via `pip install colcon-meson` and build the `libcamera` Meson project directly.**

---

# libcamera CMake wrapper

This implements a thin CMake wrapper around the [libcamera](https://libcamera.org) meson project. It only builds the main library without examples, tests or documentation.

```sh
cmake -B build -GNinja
cmake --build build
```

The number of parallel processes can be limited by setting `NJOBS`:
```sh
cmake -B build -GNinja -DNJOBS=2
cmake --build build
```

When using this as part of a colcon workspace, `NJOBS` can be set via `--cmake-args`:
```sh
colcon build --cmake-args="-DNJOBS=2"
```
