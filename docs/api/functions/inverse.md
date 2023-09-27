---
title: Inverse
layout: page
nav_order: 3
parent: Functions
grand_parent: API Reference
---

## Inverse

**Description:** Inverse of a positive definite rank 2 Tensor (Determinant > 0). Optional Argument: Determinant

| Property   | Value                  |
| ---        | ---                    |
| Result     | Tensor-valued function |
| Data Types | `Tensor2`, `Tensor2s`  |

### Example

```fortran
type(Tensor2)  :: C, invC
! type(Tensor2s) :: C, invC
real(kind=8)   :: detC

invC = inv(C)

detC = det(C)
invC = inv(C, detC)
```