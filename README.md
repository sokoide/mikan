# Mikan

## About

* This is a record of MikanOS excercises explained in <www.amazon.co.jp/dp/B08Z3MNR9J> on Apple Silicon mac
* Patched mikanos-build as described in <https://qiita.com/yamoridon/items/4905765cc6e4f320c9b5>

## Setup

```bash
# llvm
brew install llvm
# set llvm paths

# instll qemu
brew install qemu

# other tools
brew install nasm dosfstools binutils
export PATH=/opt/homebrew/sbin:/opt/homebrew/opt/binutils/bin:$PATH

# EDK II
git clone https://github.com/tianocore/edk2.git
cd edk2
# use stable202011 branch to avoid 'Instance of library class [RegisterFilterLib] is not found'
git checkout -b b_edk2-stable202011 edk2-stable202011
git submodule update --init --recursive
cd BaseTools/Source/C
CXX=llvm make -j16

# change edk2/conf/target.txt
ACTIVE_PLATFORM       = MikanLoaderPkg/MikanLoaderPkg.dsc
TARGET_ARCH           = X64
TOOL_CHAIN_TAG        = CLANGPDB

# patch
patch -p1 < patches/mac.patch
# mikanos-build ... see the Qiita link
```

