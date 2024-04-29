# Packages
sudo apt install libeigen3-dev
sudo apt-get install libglu1-mesa-dev freeglut3-dev mesa-common-dev
sudo apt-get install mesa-utils
sudo apt-get install libglew-dev
sudo apt install -y qtcreator qtbase5-dev qt5-qmake
sudo apt-get install libqt5x11extras5-dev
sudo apt-get install libv4l-dev

# Third party
git clone https://github.com/laurentkneip/opengv
mkdir build && cd build && cmake .. && make && sudo make install

# Corrections
- replace `find_package(CUDAToolkit REQUIRED)` with:
```
find_package(CUDA REQUIRED)
add_library(cuda INTERFACE)
set_target_properties(cuda PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES ${CUDA_INCLUDE_DIRS}
    INTERFACE_LINK_LIBRARIES "${CUDA_LIBRARIES};${CUDA_CUFFT_LIBRARIES};${CUDA_CUBLAS_LIBRARIES}"
)

SET(CUDA_HOST_COMPILATION_CPP ON)
```
- replace `CUDA::cublas` with `cuda` in all cmakelists

cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-11.8/ -DCMAKE_CUDA_FLAGS="-arch=sm_61" ..
make -j3 camera_calibration