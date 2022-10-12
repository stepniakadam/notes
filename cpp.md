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

## What problems C++20 modules aim to solve?

C++ creates challanges to developers when codbase grows:
* Compilation slows down when code is not structured properly
* Unnecessary complation, release and testing as a result of inproper dependency management
* Violation of ODR may happen resulting with hard-to-detect problems
* Static Initialization Fiasco (not entirely depended on codbase size)

As a result:
* Developer should know compilation phases
* Developer should know linkage types (internal, external)
* Developer should know how proper way structuring their code into *.cpp and *.hpp files

## What developer should know about compilation phases?

Maintaining consistent and well organized physical code structure was challanging until C++20 module come to save our souls. Back days it was necessary for developer to know basic compilation and linking phases in order to mimic 'modules'. Determining how frequently used headers are 'polluted' by unncessary inclusions was the only way of fighting for quick workflow. 

Let's assume we are assigned to develop small library consumed by many components of our system. It other words it will be included in many cpp files by other developers. Company will use the library internally, but developer and user will never consult each other about their needs. Some may think that it's against the common sense but this case covers also legacy libraries, large-scale organizations, lack of communication, needs are not clear, etc. 



It will be provided for other teams by package of a :

```
class 
```

Without knowing what compiler does for us. There were attempts of  code metrics helping developers to determine how far they are from 


## What DWARF section contains in ELF files?


## When return value optimization is guaranteed by standard?

"In the initialization of an object, when the source object is a nameless temporary and is of the same class type (ignoring cv-qualification) as the target object. When the nameless temporary is the operand of a return statement, this variant of copy elision is known as RVO, "return value optimization"." 
     
```
int execute() {
    return {};  // Guaranteed
}
```


