# Compiling for emscripten: 

emcc -DROGUEJS -O3 -Oz --emrun  --memory-init-file 1 -s ASSERTIONS=2 -lidbfs.js -s ASYNCIFY --preload-file pdcfont.bmp@/ --preload-file pdcicon.bmp@/  -o ~/t
mp/lib/libpdcurses.bc -I. pdcurses/*.c sdl1/*.c

# compiling programs using this bytecode

em++ -std=c++11 -lm  -I$HOME/tmp/include -L$HOME/tmp/lib $HOME/tmp/lib/libpdcurses.bc -O0 -g -s WASM=1 -s ALLOW_MEMORY_GROWTH=1 -s EXIT_RUNTIME=1 --use-preload-plugins --preload-file lib/terminfo@/home/web_user/.terminfo  --preload-file pdcfont.bmp@/ --preload-file pdcicon.bmp@/ --preload-file pdcback.bmp@/ -s ASYNCIFY --tracing -lSDL -o index.html sample.cpp

# newwin not working and workaround

* if the newwin size is larger than parent window, pdcurses doesn't draw the
  window. use newwin(0,0,0,0) to expand window as much as parent
