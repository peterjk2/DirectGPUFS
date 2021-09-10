CUDA_DIR=/usr/local/cuda
LIBCUDA=-L$(CUDA_DIR)/lib64 -lcudart -lcudadevrt
CUDAINC=-I$(CUDA_DIR)/include
CC=/usr/bin/g++

GPUFSDIR?=../../libgpufs/
REG?=32

CXXFLAGS = -std=c++11
GPUFSINC = $(GPUFSDIR)/../gpufs_con_lib.user

OBJS = matrixMul_kernel.o matrixMul.o

MAIN = mm

ifneq ($(dbg),1)
	MACROS=-DDEBUG_NOINLINE="" -DBLOCK_SIZE="32"
	CUFLAGS = --std=c++11 -O3 -rdc=true -maxrregcount=$(REG) -gencode arch=compute_80,code=sm_80
	LINKFLAGS = -O3 -rdc=true -gencode arch=compute_80,code=sm_80
	NVCC=$(CUDA_DIR)/bin/nvcc $(CUDAINC) $(MACROS)
	GPUFSLIBS = -lgpufs -L$(GPUFSDIR)/release
else
	MACROS=-DDEBUG_NOINLINE="__noinline__" -DBLOCK_SIZE="32" -DMALLOC_STATS -DTIMING_STATS -DDEBUG
	CUFLAGS = --std=c++11 -O0 -rdc=true -gencode arch=compute_80,code=sm_80
	LINKFLAGS = -O0 -rdc=true -gencode arch=compute_80,code=sm_80
	NVCC=$(CUDA_DIR)/bin/nvcc $(CUDA_INCLUDE) $(MACROS)
	GPUFSLIBS = -lgpufsd -L$(GPUFSDIR)/release
endif

all: $(MAIN)

$(MAIN): $(OBJS)
	$(NVCC) $(LINKFLAGS) $(GPUFSLIBS) -o $(MAIN) $(OBJS) $(LDLIBS) $(LIBCUDA)

matrixMul.o: matrixMul.cu
	$(NVCC) $(FLAGS) $(CUFLAGS) -I$(GPUFSINC) -I$(GPUFSDIR) -c $<

matrixMul_kernel.o: matrixMul_kernel.cu
	$(NVCC) $(FLAGS) $(CUFLAGS) -I$(GPUFSINC) -I$(GPUFSDIR) -c $<

clean:
	\rm -f $(MAIN) *.o *~

