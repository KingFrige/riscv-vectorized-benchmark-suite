#makefile
GCC_TOOLCHAIN_DIR := /home/stc/tools/riscv/llvm
SYSROOT_DIR := $(GCC_TOOLCHAIN_DIR)/riscv64-unknown-elf

LLVM := /home/stc/tools/riscv/llvm

SPIKE := spike
PK := pk

serial_target = bin/swaptions_serial.exe
vector_target = bin/swaptions_vector.exe

start:
	mkdir bin; \
	rm bin/*.*

serial:
	for entry in src/*.cpp ; do \
		${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -c -o  $$entry.o  $$entry ; \
	done
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -O2 -fno-vectorize -o $(serial_target) src/*.cpp.o -lm;	\
	rm src/*.o; \
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c -ds $(serial_target) > bin/swaptions_serial.dump;

vector:
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/CumNormalInv.o src/CumNormalInv.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/MaxFunction.o src/MaxFunction.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/RanUnif.o src/RanUnif.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/nr_routines.o src/nr_routines.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/icdf.o src/icdf.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/HJM_SimPath_Forward_Blocking.o src/HJM_SimPath_Forward_Blocking.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/HJM.o src/HJM.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/HJM_Swaption_Blocking.o src/HJM_Swaption_Blocking.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -c -o src/HJM_Securities.o src/HJM_Securities.cpp;	\
	${LLVM}/bin/clang++ --target=riscv64-unknown-elf -march=rv64gv0p10zfh0p1 -menable-experimental-extensions  -mllvm -riscv-v-vector-bits-min=128 --sysroot=$(SYSROOT_DIR) --gcc-toolchain=$(GCC_TOOLCHAIN_DIR) -DUSE_RISCV_VECTOR -O2 -fno-vectorize -o $(vector_target) src/*.o -lm;	\
	${LLVM}/bin/llvm-objdump  --mattr=+m,+f,+d,+a,+c,+experimental-v -ds $(vector_target) > bin/swaptions_vector.dump ; \
	rm src/*.o ;

host:

copy:

runspike-serial:
	$(SPIKE) --isa=rv64gcv $(PK) $(serial_target) -ns 8 -sm 512 -nt 1

runspike-vector:
	$(SPIKE) --isa=rv64gcv $(PK) $(vector_target) -ns 8 -sm 512 -nt 1

clean:
	rm -f bin/*
