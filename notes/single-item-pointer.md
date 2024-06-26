# Single-item Pointer #

Created on 2024-03-12; last updated on 2024-05-13

The type `*T` describes a [pointer](./pointer.md) to exactly one value of type `T`.

For any variable `x` of type `T`, the expression `&x` represents the memory address of `x` as a value of type `*T`. The presence of the keywords `const`, [`align`](./alignment.md) and [`addrspace`](./address-space.md) in the declaration of `x` influences the attributes of the type of `&x`.

For any pointer `p` of type `*T`, the expression `p.*` evaluates to the value of type `T` at the memory address held by `p`.

The dereference operator has greater precedence than the address-of operator. Thus, the expression `&p.*` is interpreted as `&(p.*)`.

Single-item pointers don't support pointer arithmetic, indexing or slicing.

The type `*T` does not coerce to the types `[*]T` and `[]T`, i.e., single-item pointers coerce to neither [many-item pointers](./many-item-pointer.md) nor [slices](./slice.md), presumably because the types have a multiplicity mismatch.

There is no special relationship between the syntactically similar types `*[]T` and `[*]T`; the former is a single-item pointer to a slice and the latter is a many-item pointer.

## Single-item pointer to an array ##

The type `*[N]T`, where `N` is a compile time-known `usize`, describes a single-item pointer to an array.

Single-item pointers to arrays support indexing and slicing. Indexing and slicing a single-item pointer to a (sentinel-terminated) array works the same way as indexing and slicing a (sentinel-terminated) array, respectively.

The type `*T` coerces to the type `*[1]T`, i.e., every single-item pointer `p` coerces to a single-item pointer to an array containing exactly one element, `p.*`.

For any single-item pointer `p` of type `*T`, the expression `p[0..1]` evaluates to a value of type `*[1]T` and the expressions `p[0..0]` and `p[1..1]` evaluate to a value of type `*[0]T`. Every other combination of (compile time-known) bounds results in a compilation error.

The type `*[N]T` coerces to the types `[*]T` and `[]T` for all compile time-known values `N` of type `usize`. In other words, every single-item pointer to an array of any size coerces to a many-item pointer and slice. In fact, even `*[1]T` coerces to `[*]T` and `[]T` but not `*T`. In accordance with these observations, we may posit that the compiler requires coercions from `*[N]T` to conserve the manyness of the source type irrespective of the value of `N`.

For any single-item pointer `p` of type `*[N]T`, the expression `p.ptr` evaluates to a `[*]T`, i.e., the corresponding many-item pointer.

The type `*[N:Z]T` coerces to the types `*[N]T`, `[*:Z]T`, `[*]T`, `[:Z]T` and `[]T` for all compile time-known values `N` of type `usize` and any sentinel value `Z` of type `T`.
