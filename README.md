# OpenFrameworks QtCreator Project for Ubuntu Linux

This project demonstrates how to compile an OpenFrameworks application using **QtCreator** on Ubuntu.

Tested with OpenFrameworks from source and QtCreator in Linux Mint 22.1 (Xia) Cinnamon.

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

(This project was aided partially by AI and passive aggressive comments found in github issues, i.e: https://github.com/openframeworks/openFrameworks/issues/6027)
