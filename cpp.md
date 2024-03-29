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

Maintaining consistent and well organized physical code structure was challanging until C++20 module come to save our souls. Back days it was necessary for developer to know basic compilation and linking phases in order to mimic 'modules'. Determining how frequently used headers are 'polluted' by unncessary inclusions was the only way of achieving what is expected for modules. 

Let's assume we are assigned to develop small library consumed by many components of the system. It other words it will be included by many cpp files. In such case it is essential to undestand compilation phases to know the impact of our decisions for rest of the system. 

Library will store and get from database containing company staff entries. Implementation assumes that call to 'add' will not store data to database immediately but will be pooled by an other object periodically for performance reasons. Quick, naive implementation may look like this:

```
#include <string>
#include "WorkersDBConnection.hpp"

class Workers {
public:
Workers(WorkersDBConnection* connection);

void add(const std::string& name);
void remove(const std::string& name);

private:
  std::vector<std::string> names;
  WorkersDBConnection* connection;
};
```

Class WorkerDBConnection was already in the sytem so we have to only implement basic 'add' and 'remove' functions. We did great job, didn't we?
Let's dive in a little bit and see what compiler does of us:

1) Preprocessing - All preprocessor directives are executed by textuall substitution.

If compiled with `-E` flag gcc stops compilation after preprocessing phase:

```
gcc -E workers.cpp | head -n 300 | tail -n 20
```

outputs:

```
(...)
    struct __is_void<void>
    {
      enum { __value = 1 };
      typedef __true_type __type;
    };

(...)
```

Head and tail commands were used to limit output to couple of lines. As we can see at the top of the file there is copy pasted definition of an struct of standard template library. Each included header was literally copy-pasted into the translation unit (cpp file). But how many lines were added into what's alredy in cpp?

```
cat workers.cpp | wc
```

outputs:
```
     15      23     211
```

There are 15 lines of worker.cpp file before preprocessor starts. But what happens after first phase of compilation?

```
gcc -E workers.cpp | wc
```

outputs:
```
  36217   78064  892068
```

There are 36217 lines after preprocessing phase! That is huge difference! This have significate impact on compilation time and we will dive deeper into that later in this article. 

2) Compilation

At this phase .cpp file including copy-pasted code from all header files (remember - this was 36217 lines of code) is used to produce binary object file. Not all symbols are defined at this point - it is allowed (and usually best practice) to only declare a symbol in *.hpp file. Compiler however will process all lines in order to determine what's needed to produce the object. 




 
## What DWARF section contains in ELF files?


## When return value optimization is guaranteed by standard?

"In the initialization of an object, when the source object is a nameless temporary and is of the same class type (ignoring cv-qualification) as the target object. When the nameless temporary is the operand of a return statement, this variant of copy elision is known as RVO, "return value optimization"." 
     
```
int execute() {
    return {};  // Guaranteed
}
```


