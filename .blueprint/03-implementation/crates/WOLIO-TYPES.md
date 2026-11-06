`.blueprint/03-implementation/crates/WOLIO-TYPES.md`
# wolio-types

## Responsibility

Define the V1 type system independently of name resolution.

## Primitive types

``` text
Bool
Int
UInt
Float
Decimal
String
Bytes
Duration
Timestamp
```

## Algebraic types

``` text
Option<T>
Result<T, E>
List<T>
Map<K, V>
```

## Semantic/domain types

V1 supports named semantic types so physically identical values remain
non-interchangeable.

Example:

``` text
type ProductPrice = Decimal
type AccountBalance = Decimal
type PaymentAmount = Decimal
```

These are distinct semantic types.

## Core API

``` rust
pub enum Type {
    Primitive(PrimitiveType),
    Named(TypeId),
    Option(Box<Type>),
    Result(Box<Type>, ErrorTypeId),
    List(Box<Type>),
    Map(Box<Type>, Box<Type>),
}
```

## Rules

-   no implicit semantic-type conversion
-   explicit conversion must be declared
-   `Result` and `Option` must be exhaustively handled
-   numeric conversion must be explicit where loss can occur

## Tests

-   type equality
-   assignability
-   semantic type separation
-   generic container typing
-   invalid conversion cases

## Non-goals

-   ownership/borrowing in V1
-   advanced generics
-   inheritance
-   reflection
