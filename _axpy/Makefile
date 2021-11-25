#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/sysroot

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/axpy_serial.exe
vector_target = bin/axpy_vector.exe

start:

serial:
	echo "serial"
	${LLVM}/bin/clang++ -Wall --target=riscv64-unknown-linux-gnu -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/axpy_serial.o src/axpy.c
	${LLVM}/bin/clang++ -Wall --target=riscv64-unknown-linux-gnu -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/main_serial.o src/main.c
	${LLVM}/bin/clang++ -Wall --target=riscv64-unknown-linux-gnu -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/utils_serial.o src/utils.c
	${LLVM}/bin/clang++ -Wall --target=riscv64-unknown-linux-gnu -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -o $(serial_target) src/*.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/axpy_serial.dump

vector:
	echo "vector"
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128  -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/utils.o src/utils.c
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128  -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/axpy.o src/axpy.c
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128  -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -c -o src/main.o src/main.c
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128  -O2  -fno-vectorize --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -o $(vector_target) src/*.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/axpy_vector.dump

host:

copy:

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) 256

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) 256

clean:
	rm -f bin/*
