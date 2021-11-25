#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/streamcluster_serial.exe
vector_target = bin/streamcluster_vector.exe

start:

serial:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/streamcluster_serial.o src/streamcluster.cpp
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/streamcluster_serial.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/streamcluster_serial.dump

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/streamcluster.o src/streamcluster.cpp
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(vector_target) src/*.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/streamcluster_vector.dump

host:

copy:

runspike-serial:serial
	echo >output.txt; \
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) 3 10 128 128 128 10 none output.txt 1

runspike-vector:vector
	echo >output.txt; \
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) 3 10 128 128 128 10 none output.txt 1

clean:
	rm -f bin/*
