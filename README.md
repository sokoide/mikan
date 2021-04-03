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

# other tools
brew install nasm dosfstools binutils
export PATH=/opt/homebrew/sbin:/opt/homebrew/opt/binutils/bin:$PATH

# EDK II
git clone https://github.com/tianocore/edk2.git
cd edk2
git submodule init
git submodule update
cd BaseTools/Source/C
make

# mikanos-build ... see the Qiita link
```

