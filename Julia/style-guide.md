# Style Guide
This doc is provided at Julia Online Docs[1]. Since Julia is a rapidly growing language, the code convention should be unstable. Just use this as a basic reference guide.

## Write functions, not just scripts
**Why:**    
1. Functions are more reusable and testable, and clarify what steps are being done and what their inputs and outputs are.    
2. Furthermore, code inside functions tends to run much faster than top level code, due to how Julia's compiler works.

## Avoid writing overly-specific types
**Example:**    
``complex(float(x))`` is better than ``Complex{Float64}(x)``
The second version will convert x to an appropriate type, instead of always the same type.

## Handle excess argument diversity in the caller
**Example:**  

```
function foo(x::Int, y::Int)
    ...
end
foo(Int(x), Int(y))
```
is better than

```
function foo(x, y)
    x = Int(x); y = Int(y)
    ...
end
foo(x, y)
```
**Why:**       
1. foo does not really accept numbers of all types.   
2. declaring more specific types leaves more "space" for future method definitions  

## Append ! to names of functions that modify their arguments
**Why:**    
Follow Julia Base's convention, like ``sort`` and ``sort!``

## Avoid strange type Unions
**Why:**    
Indicates that design might not be clean

## Avoid elaborate container types
**Why:**  
Not much help

## Use naming conventions consistent with Julia ``base/``
**Why:**  
You know the reason

## Write functions with argument ordering similar to Julia Base
**Why:**  
You know the reason

## Don't overuse try-catch
**Why:**  
It is better to avoid errors than to rely on catching them.

## Don't parenthesize conditions
**Example:**  
Use ``if a == b`` instead of ``if (a == b)``

## Don't overuse ``...``
**Why:**  
Splicing function arguments can be addictive, but don't abuse.

## Avoid confusion about whether something is an instance or a type
**Example:**.   
Following is confusing

```
foo(::Type{MyType}) = ...
foo(::MyType) = foo(MyType)
```
**How:**    
The preferred style is to use instances by default, and only add methods involving Type{MyType} later if they become necessary to solve some problem.

## Don't overuse macros
**Why:**    
Calling eval inside a macro is a particularly dangerous warning sign; it means the macro will only work when called at the top level. If such a macro is written as a function instead, it will naturally have access to the run-time values it needs.

## Don't expose unsafe operations at the interface level
**How:**    
Check the operation to ensure it is safe, or have unsafe somewhere in its name to alert callers. 

## Don't overload methods of base container types
**Why:**    
The trouble is that users will expect a well-known type to behave in a certain way, and overly customizing its behavior can make it harder to work with.

## Avoid type piracy
**Why:**    
"Type piracy" refers to the practice of extending or redefining methods in Base or other packages on types that you have not defined. In some cases, you can get away with type piracy with little ill effect. In extreme cases, however, you can even crash Julia.

## Be careful with type equality
**How:**    
Use ``isa`` and ``<:`` for testing types, not ``==``

## Do not write x->f(x)
**Example:**    
Use ``map(f, a)`` instead of ``(x->f(x), a)``

## Avoid using floats for numeric literals in generic code when possible
**How:**    
Use ``Int`` literals when possible, with ``Rational{Int}`` for literal non-integer numbers, in order to make it easier to use your code.  

# Reference
https://docs.julialang.org/en/v1/manual/style-guide/