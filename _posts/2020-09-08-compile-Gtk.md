---
title: Compile GTK
key: 2020-09-08
tags: Plug-in
---

### 1. Compile
##### 1. Getting GTK
`git clone https://github.com/GNOME/gtk.git`   
##### 2. GTK versions
```shell
git branch -a
git checkout 3.12.2
git clean -xdf
```
##### 3. Compiling
```shell
cd ~/gtk
./autogen.sh
./configure
make
find . -name "*.so*" -exec cp {} ~/gtk_src/ \;
```
##### 4. Set GTK
Simply use the LD_LIBRARY_PATH environment variable and point it to your .libs folder that you got from compilation.   
```
 LD_LIBRARY_PATH
 /home/ericwill/src/gtk_versions/3-14/gtk/gtk/.libs
 ```

### 2. Issues
##### 1. No GTK-Doc found, please install it
`sudo apt-get install gtk-doc-tools`
##### 2. No package 'gdk-pixbuf-2.0' found
`sudo apt-get install libgtk2.0-dev`
##### 3. 'pkg-config --modversion glib-2.0' returned 2.46.2, but GLIB (2.42.1)
Change LD_LIBRARY_PATH to include the path of newly compiled glib
##### 4. No package 'atk' found
1. check atk by command: pkg-config --libs atk, brew list atk
2. install atk: brew uninstall atk && brew install atk
3. check PKG_CONFIG_PATH: pkg-config pkg-config --variable pc_path
4. add the new atk locaiton to PKG_CONFIG_PATH env 

##### 5. ./autogen.sh: 10: ./autogen.sh: autoreconf: not found
`sudo apt-get install dh-autoreconf`

----

[SWT/Devel/Gtk/Dev guide](https://wiki.eclipse.org/SWT/Devel/Gtk/Dev_guide)   
[SWT/Devel/Gtk/Compiling Gtk](https://wiki.eclipse.org/SWT/Devel/Gtk/Compiling_Gtk)   
[Attempting to install gtk+3 fails on atk dependency](https://github.com/Homebrew/linuxbrew-core/issues/7175)  


