### Cpp notes

## Modules compilation process


## How to verfiy if a symbol of a object/binary has external or internal linkage?

According to cppreference:
"external linkage - The name can be referred to from the scopes in the other translation units".

Standard does not define any ABI or how operation system should define an object file in order to fulfill the requirement. The only abstraction on this level is a linkage that happens between translation units. Translation units are result of compilation of *cpp files into a object that has platform specific format. 

Modern Unix systems use ELF (Executable and Linkable Format) files in three forms:
* relocatable object file
* executable object file
* shared object file

Each relocatable object file knows what symbols it defines and what symbols it uses and has not definition to. The global sybols that are used by the object but does not defined are called 'externalls' and can be view in Linux by executing:
```
nm -g file.o
```
Cpp stardard defines conditions for external linkage but it does not definene underlying binary and linker mechanis. It only affects on how it's implemented. Examples of names that will have external linkage are:
* functions not declared static
* non-const variables not declared static

This means that for given file.cpp:
```
int var1;
void execute() {}
```

There will be two external linkage *names* and as a result the relocatable object file will have these symbols marked as external. This can be checked by executing ```nm -g file.o``` and will have output:
```
0000000000000000 B var1
0000000000000000 T _Z7executev
```

## What DWARF section contains in ELF files?




