# SVML

在一年前我们刚换到现在系统上的时候用 ICC 经常会碰到 `svml` undefined reference（DPCPP 貌似修好了这个问题），可是每次检查的时候总是找得到对应的链接，是 intel compute library 里面的某个库。

查了一下这个是 [intel](https://www.intel.com/content/www/us/en/develop/documentation/cpp-compiler-developer-guide-and-reference/top/compiler-reference/intrinsics/intrinsics-for-short-vector-math-library-ops.html) 的 `Intrinsics for Short Vector Math Library Operations`。 AMD 暂时没有，可是应该后端对应的 avx2 指令是有的。后来发现是 cmake 会检查是否是 Intel 机器默认开启某个选项，遇到问题了去链接那个文件就好了。