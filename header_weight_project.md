#### Why the tool was written?

C++ is fameous for it's notorius issues with compilation time. When the code is not designed with build performance in mind then there is a graduall degradation in build time. When a build time is slow then each change in a module/library takes longer. If the module/library changes frequently and the team is large then each minutes spent on waiting until the build completes costs large amount of money. 

Suppose you work in a team of 10 people having build time increase of 1 minute. Let's assume conservative estimate that each developer triggers build 20 times a day. It means that one developer will wait additional 20 minutes a day on waiting. 10 developers will spend 200 minutes on wait in each day and 200 minutes x 7 days = 1400 minutes = 23.3 hours. It const the team almost 3 man-days of waiting at each 1 minutes of compilation slow down!

One may say that when code compiles a developer could start antoher task in parallel. This is true to some extend but context switches are not free either in computer science and in human nature. When switch context is needed each build then it will cost productivity drop and mental exhaustion. Another thing is that when compilation takes 1-2 minutes in total then there is no room for switch at all. Each additional minutes will be spend on stall. 

There are many reasons why compilation time slows down but generally speaking when there is more source to compile then time increases. Keeping translation units as slim as possible will help to increase build performance. But how to achieve that? Should I focus on writting less code in order to achieve that?
Fortunately there is another, better way.

#### Why number of includes affect compilation time?

There are following main phases of comilation:
- preprocessing
- compiling
- assembling
- linking

In the preprocessing phase each included header file is copy-pasted into *.cpp file and textually preprocessed. At this stage a C++ file can grow significantelly. For example:

```
int main() {
	int i = 0;
	i++;
	return i;
}
```
has only 12 lines o code after preprocessing and takes about 80ms to compile.

```
#include <iostream>
int main() {
	int i = 0;
	i++;
	return i;
}
```
has 31963 lines of code after preprocessing and takes about 500ms to compile.

```
#include <iostream>
#include <vector>
#include <array>
#include <map>
#include <thread>

int main() {
	int i = 0;
	i++;
	return i;
}
```
has 48234 lines of code after preprocessing and takes about 670ms to compile. 

```
Large number headers lines included = Slow compiltion time
```

#### What metrics are useful for debugging slow compilation time?

* Translation unit compilation time - Why? If it's not measured it's hard to know if there is any improvemnt.
* Number of header lines included - Why? Compilation time is proportional to number of header lines included.
* Developer that included the file - Why? Each dev should know the impact of it's inclusions. 
* Number of cpp files the header is included - Why? When all cpp files consist of the biggest header then there is a room for improvement.

#### What summaries are available in the tool?

* Compilation time summary - it's aimed to detect translation units compiling slowly 
```
<cpp path> | <compilation time> | <number of header lines> +
	<header 1> | <dev id> | <number of lines> | <included in x cpp files>
	<header 2> | <dev id> | <number of lines> | <included in x cpp files>
	...
	<header n> | <dev id> | <number of lines> | <included in x cpp files>
```

* Header inclusion summary - it's aimed to detect large headers that may be overincluded
```
<hpp path> | <number of lines> | <included in x cpp files> +
	<header 1> | <dev id> | <number of lines> | <included in x cpp files>
	<header 2> | <dev id> | <number of lines> | <included in x cpp files>
	...
	<header n> | <dev id> | <number of lines> | <included in x cpp files>
```



