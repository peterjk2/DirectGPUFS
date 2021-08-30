CUDA_DIR=/usr/local/cuda
LIBCUDA=-L$(CUDA_DIR)/lib64 -lcudart -lcudadevrt
CUDAINC=-I$(CUDA_DIR)/include
CC=/usr/bin/g++
GPUFSDIR?=../../libgpufs

CXXFLAGS=-fopenmp -std=c++0x

CUFLAGS =--std=c++11 -O3 -rdc=true -maxrregcount=32 -gencode arch=compute_75,code=sm_75
LINKFLAGS = -O3 -rdc=true -gencode arch=compute_75,code=sm_75
NVCC=$(CUDA_DIR)/bin/nvcc $(CUDAINC)

GPUFSINC = $(GPUFSDIR)/../gpufs_con_lib.user
GPUFSLIBS = -lgpufs -L$(GPUFSDIR)/release

OBJS =	fs.o

MAIN = fs

all: $(MAIN) \
	 img_cpu

$(MAIN): $(OBJS)
	$(NVCC) $(LINKFLAGS) $(GPUFSLIBS) -o $(MAIN) $(OBJS) $(LDLIBS) $(LIBCUDA)

img_cpu: img_cpu.o
	$(CC) $(CXXFLAGS) -o $@ $<

img_cpu.o: img_cpu.cpp
	$(CC) $(CXXFLAGS) -c $<

fs.o: fs.cu
	$(NVCC) $(CUFLAGS) -I$(GPUFSDIR) -I$(GPUFSINC) -c $<

clean:
	\rm -f $(MAIN) *.o *~
