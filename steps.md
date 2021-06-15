## Building Open3D on RPi 64-bit OS (aarch64)

Below are the steps that I followed:-

### Install dependencies

```
sudo apt-get update -y
sudo apt-get install -y apt-utils build-essential git cmake
sudo apt-get install -y python3 python3-dev python3-pip
sudo apt-get install -y xorg-dev libglu1-mesa-dev
sudo apt-get install -y libblas-dev liblapack-dev liblapacke-dev
sudo apt-get install -y libsdl2-dev libc++-7-dev libc++abi-7-dev libxi-dev
sudo apt-get install -y clang-7
```

### Build

```
# Clone
git clone --recursive https://github.com/intel-isl/Open3D
cd Open3D
git submodule update --init --recursive
mkdir build
cd build
```


```
# Configure
cmake \     
    -DCMAKE_BUILD_TYPE=Release \     
    -DBUILD_SHARED_LIBS=ON \     
    -DBUILD_CUDA_MODULE=OFF \     
    -DBUILD_GUI=OFF \     
    -DBUILD_TENSORFLOW_OPS=OFF \     
    -DBUILD_PYTORCH_OPS=OFF \     
    -DBUILD_UNIT_TESTS=ON \     
    -DCMAKE_INSTALL_PREFIX=~/open3d_install \     
    -DPYTHON_EXECUTABLE=$(which python3) \    
    -DUSE_BLAS=ON \    
    -DBUILD_FILAMENT_FROM_SOURCE=ON \    
    -DCMAKE_CXX_FLAGS_RELEASE=ON \    
    -DCMAKE_CXX_FLAGS_RELEASE="-O3 -DNDEBUG -faligned-new"  \    ..

```

```
# Build C++ library
make -j$(nproc)
```

```
# Install Open3D python package (optional)
make install-pip-package -j$(nproc)
python -c "import open3d; print(open3d)"
```
