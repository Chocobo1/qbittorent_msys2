# qbittorent_msys2

### How to Compile [qBittorrent][qbittorrent-link] in [MSYS2][msys2-link]

This guide is to help developers setup development environment quickly.

**Warning!** the compilation will require ~10 GB of free space to compile & run qBittorrent (dynamically linked).

## Steps:
### 1. Install msys2
Refer to https://www.msys2.org/

### 2. Download PKGBUILD
Depending on your intention, download one PKGBUILD from the links below and put it into a clean folder.
* Build development source \
  https://raw.githubusercontent.com/Chocobo1/qbittorent_msys2/master/PKGBUILD

* Build stable release \
  Download every file in this folder: https://github.com/msys2/MINGW-packages/tree/master/mingw-w64-qbittorrent \
  To download each file, click the filename then on the right side click `Raw` button and <kbd>Ctrl</kbd> + <kbd>s</kbd> to save the file.

### 3. Install Dependencies & Build!
If you want to build a x64 application then open `MSYS2 MinGW 64-bit` in Windows start menu.
Otherwise open `MSYS2 MinGW 32-bit`.

First make sure msys2 is up-to-date by running the following command **a few times**, until it tells you all packages are latest:
```shell
pacman --sync --refresh --sysupgrade
```

Install development tools:
```shell
pacman --sync --noconfirm autoconf automake binutils make mingw-w64-x86_64-gcc
# run the below command if you need to build 32-bit
pacman --sync --noconfirm mingw-w64-i686-gcc
```

Start building qBittorrent:
```shell
cd <PKGBUILD_directory>
# build 64-bit qBittorrent 
makepkg --skippgpcheck --syncdeps --noconfirm
# build 32-bit qBittorrent
makepkg --skippgpcheck --syncdeps --noconfirm
```

### 4. Install & Run
After the command complete, you should see it created package: `mingw-w64-x86_64-qbittorrent-4.0.1-1-any.pkg.tar.xz`. The architecture & version number may be different.

Install:
```shell
pacman -U mingw-w64-x86_64-qbittorrent-4.0.1-1-any.pkg.tar.xz
```

Run (in the same console):
```shell
qbittorrent
```

You can find the downloaded qBittorrent source code in `<PKGBUILD_directory>/src/qbittorrent`.

### References:
* https://wiki.archlinux.org/index.php/pacman
* https://wiki.archlinux.org/index.php/PKGBUILD


[qbittorrent-link]: https://github.com/qbittorrent/qBittorrent
[msys2-link]: https://github.com/Alexpux/MINGW-packages
