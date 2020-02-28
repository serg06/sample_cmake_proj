To start coding:
	- Clone this project
	- Edit CMakeLists.txt and set vars (ctrl+f "TODO")
	- `mkdir build`
	- `cd build`
	- `cmake .. [-G generator-name]`
	- Put source files into src/
	- Put include files into include/
	- Put and compiled libraries into lib/
	- `cmake .. [-G generator-name]`

Adding features:
	- To add projects that your project relies on, use `add_library(proj_name file1 file2 ...)`
	- To add a precompile header, use `target_precompile_headers(${PROJECT_NAME} file1 file2 ...`
	- To link libraries, use `target_link_libraries(${PROJECT_NAME} [debug libname1_d optimized] libname1 [debug libname2_d optimized] libname2 ...)`
	- To change behaviour based on if building for 64-bit or 32-bit, use `if (BUILDING_64_BIT)`

Notes:
	- Only tested on Windows with Visual Studio
