# Boost

[website](https://www.boost.org/)

## Spack

```bash
spack info boost
spack install boost
```

## Source

```bash
./bootstrap.sh --help
# Select your configuration options and invoke ./bootstrap.sh again without the --help option. Unless you have write permission in your system's /usr/local/ directory, you'll probably want to at least use

./bootstrap.sh --prefix=path/to/installation/prefix
# to install somewhere else. Also, consider using the --show-libraries and --with-libraries=library-name-list options to limit the long wait you'll experience if you build everything. Finally,

./b2 install
# will leave Boost binaries in the lib/ subdirectory of your installation prefix. You will also find a copy of the Boost headers in the include/ subdirectory of the installation prefix, so you can henceforth use that directory as an #include path in place of the Boost root directory.

# and add to PATH and LD and INCLUDE
```

## 版本相关问题

This is Version 3 of the Filesystem library. Version 2 is not longer supported. 1.49.0 was the last release of Boost to supply Version 2。