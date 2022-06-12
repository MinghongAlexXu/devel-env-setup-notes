```shell
apt-add-repository ppa:fish-shell/release-3
add-apt-repository ppa:git-core/ppa
add-apt-repository ppa:xmake-io/xmake
add-apt-repository ppa:plt/racket
# To get latest CMaeke, follow https://apt.kitware.com/ e.g.
# wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
# apt-add-repository 'deb https://apt.kitware.com/ubuntu/ focal main'

git config --global user.name "Minghong Xu"
git config --global user.email "86758413+MinghongAlexXu@users.noreply.github.com"
git config --global core.editor "code --wait"
git config --list

# Install prompt and plugin mngr, starship and fisher
# Install Rust
fish_add_path $HOME/.cargo/bin
# Install sdkman
fisher install reitzig/sdkman-for-fish  # makes sdkman usable from fish

apt install build-essential
add-apt-repository -y ppa:ubuntu-toolchain-r/test
apt install gcc-11 g++-11
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 100 --slave /usr/bin/g++ g++ /usr/bin/g++-11 --slave /usr/bin/gcov gcov /usr/bin/gcov-11
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 80 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9

wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | sudo apt-key add -
apt-add-repository "deb http://apt.llvm.org/focal/ llvm-toolchain-focal-14 main"
apt update
apt install clang-14 lldb-14 lld-14 clangd-14
update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-14 100  # makes it the default clangd
```

To review what Personal Package Archives (PPA) are currently enabled on the system, see `/etc/apt/sources.list.d/`.
