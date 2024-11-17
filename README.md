# xdemo
Skeleton of cmake with pitchfork project layout

I am moving from using makefiles to cmake. I created
this minimal project as a training execies. Today
it reflects normal workflow I am accustomed to: build,
lint, test, check coverage.

### Commands I tried

| Command          | Description |
|:-----------------|:--------|
| rm -rf build | wipe out current build |
| cmake -S . -B build | generate new setting in build |  
| cmake --build build | build everyhing except doxygen |  
| cmake --build build --target docs | build doxygen |  
| ctest -T test --test-dir build | run tests |  
| cmake --build build --target lib_lcov | code coverage for the library |
| cmake --build build --target app_lcov | coverage for the app |
