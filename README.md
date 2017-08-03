Login Server
============

Sylverant login server.

This is [sylverant](http://sourceforge.net/projects/sylverant/) fork focused on better Blue Burst support.


Building
========


```
git clone https://github.com/isage/sylverant-login_server --recursive
cd sylverant-login_server
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/usr ..
make
```

**Note the --recursive in clone command**