# Building tamatool-vita

0. Install [vitasdk](https://vitasdk.org/) on your system, as well as cmake
1. Clone this project
   - `git clone --recursive https://github.com/nn9dev/tamatool-vita`
2. Run build commands
```shell
   cd tamatool-vita/vita
   mkdir build
   cd build
   cmake ..
   make
```
3. ???
4. The produced `tamatool-vita.vpk` should be in the `build` directory which you are currently in (run `ls`).


### Notes
There is an included CMakeLists.txt in the root, all it does is add the vita directory as a submodule. This is for convenience.

The `copy_self.sh` script is for copying the eboot.bin over to a Vita3K installation. It helped make my life a little less painful, so I'm keeping it in.