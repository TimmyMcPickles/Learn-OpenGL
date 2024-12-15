# Learn-OpenGL
My first steps towards becoming a graphics programmer.

Using [Learn OpenGL](https://learnopengl.com/) to learn the ropes of OpenGL

## Linux Build Instructions
1. Use your distros build system to install `cmake`, `g++`, and `git` for example:
  ```
  sudo apt-get install g++ cmake git
  ```
if you are using distros such as Ubuntu or Debian (Might add more package managers later). Also install `make` if you don't have it already.

2. Clone the repo into whichever directory you'd like, and build:
  ```
  git clone https://github.com/TimmyMcPickles/Learn-OpenGL.git
  cd Learn-OpenGL
  mkdir build && cd build
  cmake ..
  make
  ```
This part of the process will create the build directory, and then build the project for you.

3. After you build the repo, simply `cd` into the `/bin/` directory and run `./app`:
  ```
  cd ..
  cd bin/
  ./app
  ```
Now the program should run!
