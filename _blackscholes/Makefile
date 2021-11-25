#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/sysroot

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/blackscholes_serial.exe
vector_target = bin/blackscholes_vector.exe

start:

serial:
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu  -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/blackscholes_serial.o src/blackscholes.cpp
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu  -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/blackscholes_serial.o -lm
	rm src/*.o
	${LLVM}/bin/llvm-objdump --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/blackscholes_serial.dump

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu  -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o src/blackscholes.o src/blackscholes.cpp ; \
	${LLVM}/bin/clang++ --target=riscv64-unknown-linux-gnu  -DUSE_RISCV_VECTOR -march=rv64gv0p10zfh0p1 -menable-experimental-extensions -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(vector_target) src/*.o -lm ; \
	rm src/*.o ; \
	${LLVM}/bin/llvm-objdump --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/blackscholes_vector.dump ;

host:

copy:

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) 1 ./input/in_64K.input prices.txt

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) 1 ./input/in_64K.input prices.txt

clean:
	rm -f bin/*
