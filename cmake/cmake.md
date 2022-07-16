# Get started with CMake 
Make sure that Qt has both `CMake` and `Ninja` installed as tools. In the graphic installer, it looks like this. 

![lorem ipsum](diagrams/gui.png)

Already installed Qt and didn't include these tools? Go to your installation directory of Qt and run `MaintenanceTool`. This will get you to the graphical installer where you can add `CMake` and `Ninja`. 

## CMake in QtCreator 
If you successfully installed `CMake` and `Ninja` through the graphical installer, no further setup in QtCreator should be required as the kit is already detected, and its flags already set. If there is an installation issue, consult this [doc](https://doc.qt.io/qtcreator/creator-project-cmake.html) and this [doc](https://doc.qt.io/qtcreator/creator-build-settings-cmake.html). 

### Adding files to a CMake project in QtCreator
Unfortunately, right clicking on the project name, going through the add new file dialog will only write to disk and will not automatically write to `CMakeLists.txt`. You will have to manually add the file names to the `add_executable` option. 

## QMake to CMake conversion 
QMake uses `.pro` files to compile projects but CMake uses `CMakeLists.txt` to compile projects. This means that your qmake projects will need to be convereted to cmake projects by creating a `CMakeLists.txt` file to replace your `.pro` file. 

Generally `CMakeLists.txt` will follow this format 
```
cmake_minimum_required(VERSION 3.14)

project(project_name LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Edit from this point onwards 

find_package(Qt6 COMPONENTS Core)
find_package(Qt6 COMPONENTS Gui)
find_package(Qt6 COMPONENTS OpenGL)
find_package(Qt6 COMPONENTS Widgets)
find_package(glm)

add_executable(project_name
  main.cpp 
  header_file_1.h code_file_1.cpp
  header_file_2.h code_file_2.cpp
)

target_link_libraries(project_name PUBLIC
    Qt::Core
    Qt::Gui
    Qt::OpenGL
    Qt::Widgets
)
```
**Note:** many references to `Qt` can also be replaced by `Qt${QT_VERSION_MAJOR}` for more specificity. e.g. `Qt${QT_VERSION_MAJOR}::Core`

The main things you will have to edit are
 * `find_package` - QtCreator detects external libraries with this command. This also refers to Qt packages such as `Gui` or `OpenGL`. Use this command to include libraries in your project. 
    * ex: `find_package(Qt6 COMPONENTS Core)`
    * This [repo](https://github.com/g-truc/glm) contains a folder `glm`. Placing this folder in the same directory as CMakeLists.txt with `find_package(glm)` in `CMakeLists.txt` should give the project access to the `glm` library. 
 * `add_executable` must have all the `.h` and `.cpp` files in the project included. QtCreator will also not see and files not included in this. 
 * `target_link_libraries` links the project with the installed QtLibraries so that `find_package` knows where to look
    * ex: `target_link_libraries(project_name PUBLIC Qt::Core)`

## Qt Examples Folder
There is a high chance that this minimalist version of your `CMakeLists.txt` is not enough to successfully build the project. In those cases consider consulting example projects that Qt has distributed. In your installation folder of Qt there should be a folder `Examples` which contains source code and more importantly `.pro` and `CMakeLists.txt` files for most aspects of Qt. A link to these examples are also available [here](https://code.qt.io/cgit/qt/qtbase.git/tree/examples?h=6.2). 
