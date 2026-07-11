# RBDOOM-3-BFG & TrenchBroom Windows Setup Guide

This guide outlines the updated steps to set up a custom compiled RBDOOM-3-BFG environment, configure it for mapping with TrenchBroom, and manage the necessary runtime dependencies.

## 1. Prerequisites
*   Download and install [Visual Studio 2022 Community Edition](https://visualstudio.microsoft.com/vs/community/).
*   Install the latest [CMake](https://cmake.org/download/) and ensure `cmake.exe` is added to your global or user PATH.
*   Download and install the latest [Vulkan SDK from LunarG](https://www.lunarg.com/vulkan-sdk/).
*   Download ISPC from the [ISPC GitHub Releases page](https://github.com/ispc/ispc/releases) and unpack the binary exactly to `DoomCode/tools/ispc/bin/ispc.exe`.

## 2. Compiling the Engine
*   **Clone the Repository:** Run `git clone --recursive https://github.com/RobertBeckebans/RBDOOM-3-BFG.git DoomCode`
*   **Generate Projects:** Navigate to the `DoomCode/neo/` folder and double click a matching configuration batch file, such as `cmake-vs2022-win64-no-ffmpeg.bat`.
*   **Compile:** Open the generated `DoomCode/build/RBDoom3BFG.sln` in Visual Studio 2022 and compile your desired configuration.

## 3. Game Data Installation
*   Locate your downloaded retail game directory (e.g., `SteamLibrary\steamapps\common\DOOM 3 BFG Edition\`).
*   Copy the `base` folder containing all your game assets.
*   Paste this `base` folder directly into your compiled game folder (e.g., `<custom compiled root>\build\Debug` or `<custom compiled root>\build\Release`).
*   Download the official release archive from [ModDB](https://www.moddb.com/mods/rbdoom-3-bfg) and extract the `base/*.pk4` files into this same local `base/` directory.

## 4. Necessary Runtime Dependencies & Tools
To ensure the engine and editor run correctly, you must copy several compiled files and folders into your local build directory.

**Required DLLs:**
You must copy the following `.dll` files from the official [ModDB release](https://www.moddb.com/mods/rbdoom-3-bfg) into your compiled engine's root folder:
*   `avcodec-58.dll`
*   `avdevice-58.dll`
*   `avfilter-7.dll`
*   `avformat-58.dll`
*   `avutil-56.dll`
*   `openAL32.dll`
*   `postproc-55.dll`
*   `swresample-3.dll`
*   `swscale-5.dll`

**Required Tool Directories:**
Copy the following compiled tool folders from the official toolset into your project directory:
*   **TrenchBroomBFG:** Found under `tools/trenchbroom/`, this is customized specifically for DOOM 3.
*   **Runtimedeps:** Contains required Visual Studio C++ redistributables.
*   **Optick-profiler:** A lightweight C++ profiler for games.
*   **Bfgpakexplorer:** A resource file manager.

## 5. TrenchBroom Configuration & Asset Generation
TrenchBroom cannot read DOOM 3's native `.bimage` or binarized models directly. You must extract these assets using the engine developer console (`~`).

*   **Extract Textures:** Run `exportImagesToTrenchBroom`. This decompresses `.bimage` files and saves them as `.png` files in `base/_tb/textures/`.
*   **Extract Models:** Run `exportModelsToTrenchBroom`. This saves binarized models to `.obj` files in `base/_tb/`.
*   **Extract Entity Definitions:** Run `exportFGD`. This exports entity definitions to `.fgd` files in `base/_tb/` for TrenchBroom to read.
*   **Model Zoo (Optional):** Run `makeZooMapForModels` to generate a map containing modular models, saved to `maps/zoomaps/zoo_models.map`. Loading this map can help you preview available assets and ensure models load successfully in the engine.

## 6. Configuring TrenchBroom Paths
When setting up TrenchBroom for the first time, you must configure the asset and engine paths correctly so it can locate your compiled game.

*   **Game Path:** Set the Game Path in TrenchBroom to your compiled game directory (e.g., `<custom compiled root>\build\Debug` or `<custom compiled root>\build\Release`).
*   **Base Folder Validation:** Ensure your `base` folder is inside the compiled game folder outlined above.
*   **Engine Configuration:** Under the "Configure engines" menu, point the executable to your new custom compiled game (e.g., `<custom compiled root>\build\Debug\RBDoom3BFG.exe`).

## 7. Mapping Workflow

### Starting a New Map
If you are just launching a new empty map, TrenchBroom will successfully find your game assets as long as you have run `exportImagesToTrenchBroom` and `exportModelsToTrenchBroom` in the engine beforehand. 
*Note: Give TrenchBroom a moment when first loading, as it needs to load in a fair bit of data.*

### Opening Existing Maps
If you want to load an existing DOOM 3 map, you will need to convert it to the Valve 220 format first.
*   Run `convertMapToValve220 <mapname>` in the engine console.
*   You can now open up the existing, converted map in TrenchBroom.

### Testing Your Maps
*   **Export:** Ensure you compile and export your new map from TrenchBroom.
*   **Launch Engine:** You can test your map by launching the engine directly from TrenchBroom.
*   **Compile BSP:** Once in the game console, it helps to run `dmap <mapname>` first to check over the map for errors before running.
*   **Play:** Finally, use the command `map <mapname>` to launch the level.