#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/particlefilter_serial.exe
vector_target = bin/particlefilter_vector.exe

start:
	mkdir bin; \
	rm bin/*.*

serial:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/particlefilter_serial.o src/particlefilter.c
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/particlefilter_serial.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/particlefilter_serial.dump

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -DUSE_RISCV_VECTOR --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/particlefilter.o src/particlefilter.c
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 -DUSE_RISCV_VECTOR --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(vector_target) src/*.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/particlefilter_vector.dump

host:

copy:

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) -x 128 -y 128 -z 2 -np 256

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) -x 128 -y 128 -z 2 -np 256

clean:
	rm -f bin/*

	
