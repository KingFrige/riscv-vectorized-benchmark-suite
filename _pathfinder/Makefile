#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/pathfinder_serial.exe
vector_target = bin/pathfinder_vector.exe

serial:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/pathfinder_serial.o src/pathfinder.cpp
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/pathfinder_serial.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/pathfinder_serial.dump

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/pathfinder.o src/pathfinder.cpp ; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(vector_target) src/*.o -lm ; \
	rm src/*.o ; \
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/pathfinder_vector.dump ; \

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) 32 32 out

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) 32 32 out

clean:
	rm -f bin/*
