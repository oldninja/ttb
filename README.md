# Tensor Toolbox for Modern Fortran (ttb)
*...repo under construction - toolbox will be uploaded in a few days...*

Commercial FEM software packages often offer interfaces (user subroutines written in Fortran) for custom defined user materials like UMAT in Abaqus or HYPELA2 in MSC.Marc. Unlike other scientific programming languages like MATLAB or Python Fortran is not as comfortable to use when dealing with high level programming features of tensor manipulation. On the other hand it's super fast - so why not combine the handy features from MATLAB or Python's NumPy/Scipy with the speed of Fortran? That's the reason why I started working on a simple but effective module called **Tensor Toolbox for Modern Fortran**.

It provides the following basic operations for tensor calculus (all written in double precision `real(kind=8)`):
- Dot Product `C(i,j) = A(i,k) B(k,j)` written as `C = A*B` or `C = A.dot.B`
- Double Dot Product `C = A(i,j) B(i,j)` written as `C = A**B` or `C = A.ddot.B`
- Addition / Subtraction `C(i,j) = A(i,j) + B(i,j)` written as `C = A+B` or `C = A.add.B`
- Multiplication and Divison by a Scalar `C(i,j) = A(i,j) - B(i,j)` written as `C = A-B` or `C = A.sub.B`
- Deviatoric Part of Tensor  `dev(C) = C - tr(C)/3 * Eye` written as `dev(C)`
- Transpose and Permutation of indices `B(i,j,k,l) = A(i,k,j,l)` written as `B = permute(A,1,3,2,4)`
- ...

The idea is to create derived data types for rank 1, rank 2 and rank 4 tensors (and it's symmetric variants). In a next step the operators are defined in a way that Fortran calls different functions based on the input types of the operator: performing a dot product between a vector and a rank 2 tensor or a rank 2 and a rank 2 tensor is a different function. Best of it: you don't have to take care of that.

## Basic Usage
The most basic example on how to use this module is to put the 'ttb'-Folder in your working directory and add two lines of code:

```fortran
       include 'ttb/ttb_library.f'

       program script101_ttb
       use Tensor
       implicit none

       ! user code

       end program script101_ttb
```
The `include 'ttb/ttb_library.f'` statement replaces the line with the content of the ttb-module. The first line in a program or subroutine is now a `use Tensor` statement. That's it - now you're ready to go.

## Neo-Hookean Material
With the help of the Tensor module the Second Piola-Kirchhoff stress tensor `S` of a nearly-incompressible Neo-Hookean material model is basically a one-liner:

```fortran
       S = dev(det(F)**(-2./3.)*Eye*C)*inv(C)+p*det(F)*inv(C)
```

While this is of course not the fastest way of calculating the stress tensor it is extremely short and readable. Also the second order tensor variables `S, Eye, F, C` and a scalar quantity `p` have to be created at the beginning of the program. A minimal working example is now:

...

## Elasticity Tensor
...

isochoric part:
```fortran
       C4_iso = det(F)**(-2./3.) * 2./3.* (
     *       tr(C) * identity4(inv(C))
     *     - (Eye.dyadic.inv(C)) - (inv(C).dyadic.Eye)
     *     + tr(C)/3. * (inv(C).dyadic.inv(C)) )
```
...
