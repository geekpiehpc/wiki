# Fortran
这个语言简直伤天害理，但确是笔者前老板最爱的语言，而且是f77，天灭fortran。可超算比赛 fortran 的上镜次数还挺多的，之前做天气应用的时候没敢改，现在既然有个小于 15w 行的程序，笔者尝试着修改。

PGI 编译器是一个商用编译器。后被 NV 收购，加了很多 fortran 可用的 cuda DSL。这无疑让 fortran 续命了不少。NVHPC 中的 Nvfortran 有很多编译器优化的 log 可以看。

[基本语法](http://micro.ustc.edu.cn/Fortran/ZJDing/)：
module 相当于 c 中的struct。
program(main)/function(normal function) 相当于 对function 的定义
```fortran
real function square(x)
    implicit none
    real, intent(in) :: x
    square = x * x
    return
end function square
program main
  integer :: n, i, errs, argcount
  real, dimension(:), allocatable :: a, b, r, e
  n = 1000000 
  call square(n)
end program
```

subroutine 相当于trait，需要有generic function 来实现
## OpenACC  
一个简单的加法
```
module mpoint
type point
    real :: x, y, z
end type
type(point) :: base(1000)
end module

subroutine vecaddgpu( r, n )
 use mpoint
 type(point) :: r(:)
 integer :: n
 !$acc parallel loop present(base) copyout(r(:))
 do i = 1, n
  r(i)%x = base(i)%x
  r(i)%y = sqrt( base(i)%y*base(i)%y + base(i)%z*base(i)%z )
  r(i)%z = 0
 enddo
end subroutine
```

```
#pragma acc loop gang
    for (j = 0; j < n1 * n2; j += n2) {
        k = 0;
        #pragma acc loop vector reduction(+:k)
            for (i = 0; i < n2; i++)
                k = k + a[j + i];
            atomicAdd(x, k);
```
### 效率对比
