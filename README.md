robust_equilibrium_lib_catkin
=============================

Catkin wrapper for the [RobustEquilibriumLib](https://github.com/andreadelprete/robust-equilibrium-lib).

It allows you to use catkin macros to find robust-equilibrium-lib. It uses the [qpoases_catkin wrapper](https://github.com/wxmerkt/qpoases_catkin) to provide the correct version of qpOASES without having to manually install it. To use robust-equilibrium-lib in your catkin package:

`package.xml`: 
```xml
<build_depend>robust_equilibrium_lib_catkin</build_depend>
<run_depend>robust_equilibrium_lib_catkin</run_depend>
```

`CMakeLists.txt`:
```cmake
find_package(catkin REQUIRED COMPONENTS robust_equilibrium_lib_catkin)
include_directories(${catkin_INCLUDE_DIRS})
target_link_libraries(test ${catkin_LIBRARIES})
```

And in your C++ file:
```c
#include <robust-equilibrium-lib/static_equilibrium.hh>
``` 

