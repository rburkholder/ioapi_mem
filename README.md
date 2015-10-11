This adds some code to zlib to provide in-memory unzip capability.

From the zlib home page, zlib is designed to be a free, general-purpose, legally unencumbered -- that is, not covered by any patents -- lossless data-compression library for use on virtually any computer hardware and operating system. 

At minizip, I found a reference to Justin Fletcher who "wrote a very simple implementation of a memory access method for the ioapi code". I made a few edits to handle 64 bit builds. 

ioapi_mem.h and ioapi_mem.c are placed into the main zlib build directory.  I use the following to build a custom zlib (I could probably build a separate library, but it simplifies some stuff for me to include this code):

<pre>
ln -s contrib/minizip/unzip.c unzip.c
ln -s contrib/minizip/unzip.h unzip.h
ln -s contrib/minizip/ioapi.c ioapi.c
ln -s contrib/minizip/ioapi.h ioapi.h

mv Makefile.in Makefile.in.orginal
cat Makefile.in.original | sed "s/zutil.o$/zutil.o ioapi.o ioapi_mem.o unzip.o/" > Makefile.in

./configure --64 --static --prefix=/usr/local --includedir=/usr/local/include/zlib

make
sudo make install

sudo cp ioapi.h /usr/local/include/zlib
sudo cp ioapi_mem.h /usr/local/include/zlib
sudo cp unzip.h /usr/local/include/zlib
</pre>
