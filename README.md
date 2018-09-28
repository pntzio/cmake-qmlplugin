# CMake-qmlplugin
This is a CMake module to make it easier to write QML-plugins when using CMake as build system.
By using `add_qmlplugin( ... )`, the build system will
* Compile the sources
* Copy `qmldir` and all specified QML-files to build output
* Run `qmlplugindump` on the generated plugin and produce `plugin.qmltypes` in the build output
* Add install targets which will install the plugin and all additional files to the `QT_INSTALL_QML` path

## Requirements
* `qmake` must be in `PATH`
* You must have a `org/mycompany/component` structure in your project in order for `qmlplugindump` to work.
    * Alternative, at least make the build output have that structure.
* There should be a `qmldir` file on the same level as the `CMakeLists.txt` file.

## Usage
### Syntax
```cmake
add_qmlplugin(<name>
    URI <uri>
    VERSION <version>
    SOURCES
        source1 [source2 ...]
    QMLFILES
        [qml1 qml2 ...]
    BINARY_DIR
        [<binary_dir>]
    NO_AUTORCC
    NO_AUTOMOC
)
```
#### Explanation
| Argument        | Explanation |
| ------------ |:--------------|
| `URI`        | URI used by the plugin, which is also used as a directory when installing the plugin. For example `org.mycompany.components` will be installed to `[QT_INSTALL_QML]/org/mycompany/components`. |
| `VERSION`    | Denotes the version of the plugin. |
| `SOURCES`    | Should contain all the C++ sources to be compiled with this plugin. |
| `QMLFILES`   | Should contain all QML files which should be copied to the binary directory and eventually installed to `[QT_INSTALL_QML]` directory. |
| `BINARY_DIR` | Used to specify the output directory when building. This should be the directory which is a parent to `org/mycompany/components` in order for `qmlplugindump` to work properly. If not explicitly set, it will default to `${CMAKE_CURRENT_BINARY_DIR}`. |
| `NO_AUTORCC` | Turn off automatic RCC |
| `NO_AUTOMOC` | Turn off automatic MOC |

### Example
```cmake
add_qmlplugin(components
URI org.mycompany.components
VERSION 1.0
SOURCES
    qmldir
    src/componentsplugin.h
    src/componentsplugin.cpp
    src/filemonitor/filemonitor.h
    src/filemonitor/filemonitor.cpp
QMLFILES
    qml/Dashboard.qml
    qml/TriangleButton.qml
BINARY_DIR
    ${CMAKE_BINARY_DIR}/src/qml
)
```

