# Working Version of the Kaleidoscope Demo from LLVM

**Requires a pre-built LLVM (v16.0)**

(build it with `cmake -DLLVM_ENABLE_PROJECTS=clang -DCMAKE_BUILD_TYPE=Release -G "Unix Makefiles" ../llvm`)

## Building

Adopt the LLVM path in `CMakeLists.txt` accordingly.
Then just build the project.

Start the shell with

```
cmake-build-debug-llvm-clang/toy
```
(or whatever build path you have).

Enter a function, e.g.

```
ready> def main()        
1
;
```

The output should look like

```
Read function definition:define double @main() {
entry:
  ret double 1.000000e+00
}
```

Then exit the shell with `strg+D`.
A file named `output.o` should be generated.
This can be linked with

```
clang++ output.o -o main
```

Another example is the "average" example.
define a function, e.g.

```
def average(x y) (x + y) * 0.5;
```

which produces code
```
Read function definition:define double @average(double %x, double %y) {
entry:
  %y2 = alloca double, align 8
  %x1 = alloca double, align 8
  store double %x, ptr %x1, align 8
  store double %y, ptr %y2, align 8
  %x3 = load double, ptr %x1, align 8
  %y4 = load double, ptr %y2, align 8
  %addtmp = fadd double %x3, %y4
  %multmp = fmul double %addtmp, 5.000000e-01
  ret double %multmp
}
```

Exit the shell again with `strg+D` and then you can link it to the main.cpp which calls the average function:

```
clang++ main.cpp output.o -o main
```

Then you can call `./main` and it works!