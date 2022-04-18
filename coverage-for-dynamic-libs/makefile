ONEAPI_HOME=/opt/intel/oneapi/compiler/latest/linux/
#CXX=/opt/intel/oneapi/compiler/latest/linux/bin/dpcpp
#PROFDATA=${ONEAPI_HOME}/bin-llvm/llvm-profdata
#COV=${ONEAPI_HOME}/bin-llvm/llvm-cov
CXX=clang++
PROFDATA=llvm-profdata
COV=llvm-cov

CXXFLAGS=-I. -fprofile-instr-generate -fcoverage-mapping -fPIC -Wall -Wextra -O0

DEPS = hello.h
OBJ = hello.o main.o

all: hello.exe

hello.o: hello.cpp hello.h
	$(CXX) -c -o $@ $< $(CXXFLAGS)

libhello.so: hello.o
	$(CXX) -shared -o $@ $<

hello.exe: libhello.so main.cpp
	$(CXX) -L. -lhello -Wl,-rpath=. -o $@ main.cpp $(CXXFLAGS)

clean:
	rm -rf *.out *.o *.so *.profdata *.profraw *.exe

gen-profdata: hello.exe
	LLVM_PROFILE_FILE="hello.profraw" ./hello.exe
	$(PROFDATA) merge -sparse hello.profraw -o hello.profdata

gen-cov: gen-profdata
	$(COV) report ./hello.exe -instr-profile=hello.profdata -object hello.o
