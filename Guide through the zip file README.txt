MONTY HALL SIMULATION - INSTALLATION AND RUN GUIDE
================================================

START HERE
----------
1. Extract the entire ZIP. Open the extracted Monty_Hall_Project folder.
2. Follow ONLY the section for your operating system below.
3. Run commands in a terminal. An IDE's integrated terminal is fine if it
   uses the specified shell/compiler environment.

This C++ program visualizes the generalized Monty Hall problem with 3-100
doors. It compares switching and staying using randomized trials.
It needs SDL2 to draw its window. A plain C++ compiler alone is insufficient.
The original tosubmit.cpp has been copied without changes.

CONTENTS
--------
tosubmit.cpp             Original project source code.
README.txt               This installation, IDE, and run guide.
requirements.txt         Human-readable C++ dependency list.
CMakeLists.txt           Builds the application and the bundled SDL2 source.
third_party/             Official SDL2 source archive, license, provenance.
original_binary/tosubmit Original Apple Silicon macOS executable, for reference.
VERIFICATION.txt         Checks performed when preparing this package.

The original binary is not portable to Windows, Linux, or Intel Macs. It
also depends on Homebrew library paths from the author's Mac. Rebuild from
source using the instructions below; do not use that binary as the installer.

A. macOS - SIMPLE PACKAGE-MANAGER METHOD
---------------------------------------
1. Open Terminal. Install Apple's compiler tools if not already installed:

   xcode-select --install

   Complete the installer before continuing. If the tools are already
   installed, skip this step. Verify the compiler:

   clang++ --version

2. If Homebrew is not installed, use its official installation command:

   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

   Follow the installer's "Next steps" to put brew on your PATH, then open
   a new Terminal. Check:

   brew --version

3. Install the SDL2-compatible development library:

   brew install sdl2-compat
   sdl2-config --version

   Homebrew currently provides SDL2 through sdl2-compat and installs SDL3
   underneath it automatically. The application still uses the SDL2 API.
   There is no need to rewrite the code to use SDL3.

4. Enter the extracted project directory. Replace the example path:

   cd "/path/to/Monty_Hall_Project"

   Tip: type cd followed by a space, drag the extracted folder into Terminal,
   then press Enter.

5. Compile (paste the entire following command as one line):

   clang++ -std=c++11 -O2 -I"$(brew --prefix sdl2-compat)/include" tosubmit.cpp $(sdl2-config --cflags --libs) -o tosubmit

6. Run:

   ./tosubmit

   Or choose 3 doors and 10,000 trials:

   ./tosubmit 3 10000

B. WINDOWS x64 - MSYS2 / GCC METHOD
-----------------------------------
1. Install MSYS2 from https://www.msys2.org/ using its official installer.
   These instructions assume the standard C:\msys64 location.
2. Open "MSYS2 UCRT64" from the Start menu. Use this terminal for all commands
   in this section, rather than ordinary Command Prompt or PowerShell.
3. Update packages:

   pacman -Syu

   If asked to close the terminal, close it, reopen MSYS2 UCRT64, and run
   pacman -Syu again to complete the update.

4. Install the matching compiler and SDL2 development package:

   pacman -S --needed mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-SDL2

   Accept the installation prompts. If the package manager offers an SDL2
   compatibility provider, it must provide the SDL2 headers and libraries.
   Verify:

   g++ --version
   sdl2-config --version

5. Extract the project to a convenient folder, for example:
   C:\Users\YourName\Downloads\Monty_Hall_Project
   Navigate there in MSYS2 UCRT64 (replace YourName):

   cd "/c/Users/YourName/Downloads/Monty_Hall_Project"

6. Compile:

   g++ -std=c++11 -O2 tosubmit.cpp $(sdl2-config --cflags --libs) -mconsole -o tosubmit.exe

   The -mconsole flag keeps the terminal available for printed results.

7. Run in the same terminal:

   ./tosubmit.exe 3 10000

   Keep using MSYS2 UCRT64 when launching, so Windows can find the required
   DLLs in C:\msys64\ucrt64\bin. If launching from an IDE, add that folder
   to the IDE run environment's PATH and restart the IDE if necessary.
   Do not mix a different MinGW installation with these libraries.

C. UBUNTU / DEBIAN LINUX
------------------------
1. Open a terminal and install the compiler and SDL2 development files:

   sudo apt update
   sudo apt install build-essential libsdl2-dev

2. Enter the extracted project folder:

   cd "/path/to/Monty_Hall_Project"

3. Compile and run:

   g++ -std=c++11 -O2 tosubmit.cpp $(sdl2-config --cflags --libs) -o tosubmit
   ./tosubmit 3 10000

   Use a graphical desktop session. A terminal-only remote server or a
   typical browser-based C++ compiler will not display the SDL window.

D. BUILD THE INCLUDED SDL2 LIBRARY WITH CMAKE
--------------------------------------------
This alternative builds SDL2 from third_party/SDL2-2.32.10.tar.gz. You do
not need an installed SDL2 package. CMake extracts the archive automatically
inside build/. The library is linked statically into this application;
normal platform system libraries/compiler runtimes are still required.
The library build uses no network downloads. Installing build tools or
platform development dependencies may need internet access.

macOS:
1. Install Xcode Command Line Tools (section A, step 1).
2. Install CMake 3.20+ from https://cmake.org/download/ and enable its command
   line tools, or, if you have Homebrew, run: brew install cmake
3. In the extracted project folder:

   cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
   cmake --build build --config Release --parallel 4
   ./build/tosubmit 3 10000

Windows with Microsoft Visual Studio:
1. Install Visual Studio Community or Build Tools with "Desktop development
   with C++", including MSVC, Windows SDK, and C++ CMake tools.
2. Open "x64 Native Tools Command Prompt for VS". Enter the project folder:

   cd /d "C:\path\to\Monty_Hall_Project"

3. Build and run (the default Visual Studio generator uses Release/):

   cmake -S . -B build
   cmake --build build --config Release --parallel 4
   .\build\Release\tosubmit.exe 3 10000

   If your configured generator is Ninja, the executable is instead at
   .\build\tosubmit.exe. Visual Studio's Open Folder can also load this
   CMakeLists.txt directly; select the tosubmit target.

Windows with MSYS2 UCRT64:
1. Install/update MSYS2 as in section B, then install the build tools:

   pacman -S --needed mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-cmake mingw-w64-ucrt-x86_64-ninja

2. In the project folder, from MSYS2 UCRT64:

   cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
   cmake --build build --parallel 4
   ./build/tosubmit.exe 3 10000

Ubuntu / Debian:
1. Install CMake, the compiler, and X11/OpenGL development dependencies:

   sudo apt update
   sudo apt install build-essential cmake libx11-dev libxext-dev libxrandr-dev libxcursor-dev libxi-dev libxfixes-dev libxss-dev libxinerama-dev libgl1-mesa-dev

2. From the project folder in a graphical desktop session:

   cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
   cmake --build build --config Release --parallel 4
   ./build/tosubmit 3 10000

These Linux dependencies cover the X11 desktop path for this visual program.
On a Wayland desktop, an available XWayland session can provide X11 support.
See the included SDL2 docs/README-linux.md for additional platform backends.
When changing compiler or operating system, use a fresh build directory.

E. USING AN IDE
---------------
An IDE does not automatically add SDL2 merely because the source has an
#include. The compiler must find the headers and the linker must find SDL2.

Easiest method for any IDE:
1. Open the whole extracted project folder.
2. Open its integrated terminal with the appropriate environment (on Windows,
   MSYS2 UCRT64), or use the external terminal described above.
3. Run your operating system's exact compile and run commands.

For an IDE that supports CMake (such as Visual Studio, CLion, or VS Code
with a CMake integration), open this folder as a CMake project, select an
installed matching compiler/toolchain, and build/run the tosubmit target.
This uses the bundled SDL2 source and handles include and linker settings.
Put "3 10000" in program/command-line arguments if desired.

For a manually configured GCC/Clang project:
- Use C++11 or newer and compile tosubmit.cpp.
- Add the compiler flags printed by: sdl2-config --cflags
- Add the linker flags printed by: sdl2-config --libs
- Since the code includes <SDL2/SDL.h>, the include search path must also
  contain the directory ABOVE SDL2 (e.g. /opt/homebrew/include on this Mac,
  or C:\msys64\ucrt64\include for MSYS2).
- Put linker libraries after the source/object files, as in the commands.
- On Windows, match the compiler to the SDL2 library distribution; MSVC
  cannot use MinGW libraries. Use the CMake/MSVC method for Visual Studio.

F. PROGRAM ARGUMENTS AND CONTROLS
---------------------------------
Syntax: tosubmit [number_of_doors] [number_of_trials]
Use ./tosubmit on macOS/Linux or ./tosubmit.exe in MSYS2.

No arguments: 100 doors, 10,000 trials.
Example: ./tosubmit 3 10000
Example: ./tosubmit 100 10000

Door counts are clamped to 3-100. Trial counts are clamped to 1-1,000,000.
Use positive integer arguments. There are 250 simulation trials per batch.
The door animation illustrates the process; each displayed animation is
separate from the statistical batch used for the win counts.

Hold SPACE: speed up the simulation.
ESC or close window: exit and print results to the launching terminal.
When DONE appears, close the window to print the completed results.
Closing early prints only the trials completed so far.

Expected theory: staying wins with probability 1/N; switching wins with
probability (N-1)/N under the host behavior modeled by the program.
For 3 doors: approximately 33.33% stay and 66.67% switch.
For 100 doors: approximately 1% stay and 99% switch.
These are theoretical values, not claimed measurements. Actual randomized
results vary between runs. The program seeds rand() from the current time.

G. COMMON ERRORS
----------------
"SDL2/SDL.h file not found": install the development package and use the
full command, including the include directory; or use bundled CMake.
"undefined reference" / unresolved SDL symbols: use the full SDL linker
flags after the source file, with a matching compiler/library architecture.
"sdl2-config: command not found": install the package and reopen the correct
terminal; on macOS complete Homebrew's PATH setup; on Windows use UCRT64.
"DLL missing" on Windows: launch from UCRT64 or add its bin folder to the
run environment's PATH. Recompile; do not run the supplied Mac binary.
"SDL video/window/renderer error": use a local graphical desktop with
working graphics drivers, not a terminal-only online compiler.
Results not printed yet: close the simulation window; output appears on exit.

H. OFFICIAL REFERENCES
----------------------
SDL2 installation: https://wiki.libsdl.org/SDL2/Installation
SDL2 CMake support: https://wiki.libsdl.org/SDL2/README-cmake
SDL2 source: https://www.libsdl.org/release/SDL2-2.32.10.tar.gz
Homebrew installation: https://brew.sh/
Homebrew SDL2 package: https://formulae.brew.sh/formula/sdl2-compat
MSYS2 installation: https://www.msys2.org/
MSYS2 SDL2 package: https://packages.msys2.org/packages/mingw-w64-ucrt-x86_64-SDL2
CMake: https://cmake.org/download/
