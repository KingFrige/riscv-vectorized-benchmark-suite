#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/canneal_serial.exe
vector_target = bin/canneal_vector.exe

start:
	mkdir bin ; \
	rm bin/*.*;

serial:
	for entry in src/*.cpp ; do \
		${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o $$entry.o  $$entry ; \
	done
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/*.cpp.o -lm ; \
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/canneal_serial.dump ; \
	rm src/*.o ; \

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/annealer_thread.o src/annealer_thread.cpp; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/main.o src/main.cpp; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/netlist.o src/netlist.cpp; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/netlist_elem.o src/netlist_elem.cpp; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/rng.o src/rng.cpp; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(vector_target) src/*.o -lm; \
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/canneal_vector.dump ; \
	rm src/*.o;

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) 1 100 300 input/100.nets 8

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) 1 100 300 input/100.nets 8

clean:
	rm -f bin/*
