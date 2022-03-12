``

```
cmake_minimum_required(VERSION 3.17)
project(HelloClion)

set(CMAKE_CXX_STANDARD 20)

include_directories("/home/peterchen/intel/oneapi/ipp/latest/include")
link_directories("/home/peterchen/intel/oneapi/ipp/latest/lib/intel64")

add_executable(HelloClion main.cpp Calc.cpp Calc.h)

target_link_libraries(HelloClion ippcore ipps)
```

