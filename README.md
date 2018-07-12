# nvim-ide
This a tutorial to setup nvim as a powerful ide.
## easy install
------
1. download neovim from [neovim release](https://github.com/neovim/neovim/releases), add nvim to PATH.
```
chmod u+x nvim.appimage
./nvim.appimage --appimage-extract
```
2. install plugin manager [vim-plug](https://github.com/junegunn/vim-plug).  
```
curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```
3. save [vimrc](https://raw.githubusercontent.com/chyxwzn/configFiles/master/vimrc) as .config/nvim/init.vim
4. `sudo apt-get install git gcc g++ make cmake python3 exuberant-ctags zlibc zlib1g-dev libncurses5-dev`
5. enter nvim, execute `:PlugInstall` to install all the plugins.
6. `pip3 install neovim click`.
7. setup youcompleteme
```
cd ~/.config/nvim/plugged/YouCompleteMe
python3 install.py --clang-completer
```
8. install [ripgrep](https://github.com/BurntSushi/ripgrep/releases)
9. enter nvim, execute `:UpdateRemotePlugins` and reboot nvim.

## offline install
------
1. you could tar the clean neovim and .config/nvim directory, copy to server, then untar it.
2. if you're on a ubuntu 14 server, python3 version would be not newest, and you may have no permission to install python packages. then you could build python3 by yourself.
```
#download python36 source
$ wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
$ tar -xvf Python-3.6.3.tgz
$ cd Python-3.6.3
$ ./configure --enable-optimizations --enable-shared --prefix=/home/xxx/python3/path
make -j4
make install
```
if you execute python3 now, it would fail, need add below line to you `.bashrc`
```
LD_LIBRARY_PATH=/home/xxx/python3/lib:/usr/lib:/usr/local/lib:$LD_LIBRARY_PATH
```
then you could download [greenlet-cp36](https://pypi.org/project/greenlet/#files), [msgpack-cp36](https://pypi.org/project/msgpack/#files), [neovim](https://pypi.org/project/neovim/#files) and [click](https://pypi.org/project/click/#files) package from [pypi](https://pypi.org/project)
```
pip3 install greenlet-0.4.13-cp36-cp36m-manylinux1_x86_64.whl 
pip3 install msgpack-0.5.6-cp36-cp36m-manylinux1_x86_64.whl 
pip3 install neovim-0.2.6.tar.gz
pip3 install click-6.7-py2.py3-none-any.whl
```
3. setup youcompleteme offline. download [cmake](https://cmake.org/download/) and [clang+llvm](http://releases.llvm.org/download.html#6.0.1), put in home directory. add cmake to PATH.
```
cd ~/.config/nvim/plugged/YouCompleteMe
EXTRA_CMAKE_ARGS="-DPATH_TO_LLVM_ROOT='/home/xxx/clang_llvm'" python3 install.py --clang-completer --system-libclang
```

## project setup
------
1. download [vimenv](https://raw.githubusercontent.com/chyxwzn/configFiles/master/vimenv) and [addsrc.sh](https://raw.githubusercontent.com/chyxwzn/configFiles/master/addsrc.sh), put in `~/bin`.
2. download [compdb](https://github.com/chyxwzn/compdb), put in `~/bin`
3. if you are in a large project, edit vimenv, add the source directories you need. then execute vimenv to generate project files. then it could save vim session, could locate files fast use ctrlp. and could navigate code using ctags.
4. to use youcompleteme and chromatica, we need generate `compile_commands.json`, you could try below commands to generate make log which contains complete compile commands with build flags.   
```
make V=1 kernel 2>&1 | tee make.log (make)
make VERBOSE=1 kernel 2>&1 | tee make.log (Cmake)
mmma showcommands framework/.../... 2>&1 | tee make.log (build android)
make SHELL='sh -x' (print all the shell commands)
make -w(--print-directory)
make -j32 -Bnkw -C . -f build/core/main.mk all_modules "BUILD_MODULES_IN_PATH=xxx/xxx"
```
then you could execute `compdb.py -p make.log` to generate compile_commands.json
------
finally, you could use nvim to trace code and write code fast. to explore more features, you should learn `init.vim` file, it contains all the configurations.
