# Mikan

## About

* This is a record of MikanOS excercises explained in <www.amazon.co.jp/dp/B08Z3MNR9J> on Apple Silicon mac
* Patched mikanos-build as described in <https://qiita.com/yamoridon/items/4905765cc6e4f320c9b5>

## Setup

```bash
# qemu build
brew install libffi gettext glib pkg-config autoconf automake pixman ninja

git clone https://git.qemu.org/git/qemu.git
cd qemu
git checkout 9950da284fa5e2ea9ab57d87e05b693fb60c79ce -b work
curl https://patchew.org/QEMU/161280769492.2878.8851519112088854609.malone@chaenomeles.canonical.com/mbox | git am --3way
mkdir build
cd build
../configure --prefix=/opt/qemu --target-list=x86_64-softmmu --enable-cocoa
make -j 8

# skip this step. llvm13 works fine
# (llvm9 build ... llvm11 didn't build kernel as expected)
#cd $HOME
#mkdir llvm9
#cd llvm9
#curl -L https://github.com/llvm/llvm-project/releases/download/llvmorg-9.0.1/llvm-9.0.1.src.tar.xz | tar xJ
#curl -L https://github.com/llvm/llvm-project/releases/download/llvmorg-9.0.1/clang-9.0.1.src.tar.xz | tar xJ
#curl -L https://github.com/llvm/llvm-project/releases/download/llvmorg-9.0.1/lld-9.0.1.src.tar.xz | tar xJ
#mv llvm-9.0.1.src llvm
#mv clang-9.0.1.src clang
#mv lld-9.0.1.src lld
#mkdir build
#cd build
#cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX=/opt/llvm9 -DLLVM_ENABLE_PROJECTS="clang;lld" ../llvm
#make -j 8
#sudo make install
#export PATH=/opt/llvm9/bin:$PATH

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

