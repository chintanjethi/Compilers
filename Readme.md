# Compilers Project
LLVM pass that takes python script as input and prints number of memory and non-memory instructions in basic blocks of program.

# Run

First you need to have llvm clang and cython package installed in your device

To install these packages in linux run the following commands:
```bash
sudo apt install clang llvm  
sudo apt install cython3
```

Then run these commands to see output for python_program.py file

```bash
cython3 -3 --embed -I/usr/lib/python3/dist-packages -o python_to_C.c python_program.py  #converts python program to C program

clang -S -emit-llvm python_to_C.c -o llvm_ir_python.ll -I/usr/include/python3.10    #converts C file to LLVM IR code

clang++ -shared -o MemoryInstruction.so MemoryInstruction.cpp `llvm-config --cxxflags --ldflags --libs` -fPIC   #converts C++ file to shared library (Custom Pass)

opt -load ./MemoryInstruction.so -memory-instruction-counter -enable-new-pm=0< llvm_ir_python.ll    #Applying the custom pass with opt

```





