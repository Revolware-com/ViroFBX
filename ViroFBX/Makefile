OS          = $(shell uname -s)
JAVA_DIR    = $(JAVA_HOME)
CC          = g++
SRC         = VROFBXExporter.cpp VROFbxToObjConverter.cpp VROImageExporter.cpp VROLog.cpp VROUtil.cpp main.cpp
OBJ         = $(SRC:.cpp=.o) Nodes.pb.o
CFLAGS      = -I$(JAVA_DIR)/include -I$(JAVA_DIR)/include/linux -I$(FBXSDK_DIR)/include -I$(PROTOBUF_DIR)/include -I../Libraries/tinyexr -I../Libraries -std=gnu++11 
LDFLAGS = $(FBXSDK_DIR)/lib/libfbxsdk.a $(PROTOBUF_DIR)/lib/libprotobuf-lite.a -lpthread -ldl -lz -pthread -std=gnu++11 

ifeq ($(OS), Darwin) # MacOS
FBXSDK_DIR   = ../macos/Libraries/fbx
PROTOBUF_DIR = ../macos/Libraries/protobuf
LDFLAGS      += -framework CoreFoundation
else ifeq ($(OS), Linux)
FBXSDK_DIR   = ../linux/Libraries/fbx
PROTOBUF_DIR = ../linux/Libraries/protobuf
else
$(error System "$(OS)" is not supported. Supperted systems: "Darwin", "Linux")
endif

all: options virofbx

options:
	@echo virofbx build options:
	@echo "OS         = $(OS)"
	@echo "CC         = $(CC)"
	@echo "CFLAGS     = $(CFLAGS)"
	@echo "LDFLAGS    = $(LDFLAGS)"
	@echo "SRC        = $(SRC)"
	@echo "OBJ        = $(OBJ)"
	@echo "JAVA_HOME  = $(JAVA_HOME)"
	@echo ""

Nodes.pb.o: Nodes.pb.cc
	$(CC) -c $(CFLAGS) $<

%.o: %.cpp
	$(CC) -c $(CFLAGS) $<

libvirofbx.a: $(OBJ) Nodes.pb.o
ifeq ($(OS), Darwin)
	libtool -static -o $@ $^
else ifeq ($(OS), Linux)
	ar rcs $@ $^
endif

virofbx: libvirofbx.a
	$(CC) -o $@ $^ $(LDFLAGS)

clean:
	rm -f virofbx *.a *.o *.orig *.rej

.PHONY: all options clean dist
.NOTPARALLEL: options
