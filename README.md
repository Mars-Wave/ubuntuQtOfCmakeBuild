# OpenFrameworks QtCreator Project for Linux Mint 22.1 (Cinnamon)

This project demonstrates how to compile an OpenFrameworks application using **QtCreator** on Linux Mint 22.1 (Xia) Cinnamon. Tested with OpenFrameworks from source and QtCreator 12.0+.

## Prerequisites

1. **Follow OF installing from github guide**
Follow: https://github.com/openframeworks/openFrameworks/blob/master/INSTALL_FROM_GITHUB.md
Install their dependencies and libraries.

## Project Setup

1. **Clone this repository or just copy the cmakelists.txt file**

2. **Configure `CMakeLists.txt`**:
- Uncomment/set your OpenFrameworks path:
```cmake
set(OF_ROOT /path/to/openFrameworks) # Line 7 in CMakeLists.txt
```

3. **Project Structure**:
```
MyOFApp/
├── CMakeLists.txt
├── bin/ # Built executable
├── data/ # Assets (copied automatically)
└── src/ # Source files (.cpp/.h)
```

## QtCreator Configuration

Standard configuration and builds can use gcc/g++.

## Addons Support
Includes:
- `ofxGui`
- `ofxVectorGraphics` (requires Cairo)

To add more addons:
1. Add include paths in section 2 of `CMakeLists.txt`
2. Add source files in section 4

## Troubleshooting

- **Missing Libraries**: Reinstall dependencies listed above
- **OF_ROOT Not Found**: Verify path in CMakeLists.txt/QtCreator variables
- **Cairo Errors**: Ensure `libcairo2-dev` is installed
- **Boost Errors**: Check `libboost-all-dev` installation

Tested on Linux Mint 22.1 Cinnamon (Ubuntu 22.04 base). For other distributions, adjust package names accordingly.
```
