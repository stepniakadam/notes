C++ is fameous for it's notorius issues with compilation time. When the code is not designed with build performance in mind then there is a graduall degradation in build time. When a build time is slow then each change in a module/library takes longer. If the module/library changes frequently and the team is large then each minutes spent on waiting until the build completes costs large amount of money. 

Suppose you work in a team of 10 people having build time increase of 1 minute. Let's assume conservative estimate that each developer triggers build 20 times a day. It means that one developer will wait additional 20 minutes a day on waiting. 10 developers will spend 200 minutes on wait in each day and 200 minutes x 7 days = 1400 minutes = 23.3 hours. It const the team almost 3 man-days of waiting at each 1 minutes of compilation slow down!

One may say that when code compiles a developer could start antoher task in parallel. This is true to some extend but context switches are not free either in computer science and in human nature. When switch context is needed each build then it will cost productivity drop and mental exhaustion. Another thing is that when compilation takes 1-2 minutes in total then there is no room for switch at all. Each additional minutes will be spend on stall. 





