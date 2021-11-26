# Cadence Symbols Glossary

A simple TLDR reference containing Cadence's symbols and operators and what they do. 

Just <kbd>CTRL</kbd>/<kbd>⌘</kbd> + <kbd>F</kbd> and type in the symbol you want to look up.

Please feel free to send a PR with more/better examples and symbols.

## @ (at) 

The @ symbol before a Type declaration is used to annotate whether a specific type is a Resource. Resources must therefore adhere to the Resource-specific lifecycle in Cadence (create, destroy, move). [More info](https://docs.onflow.org/cadence/language/resources/)


```cadence
// Declare a resource named `SomeResource`
pub resource SomeResource {
    pub var value: Int

    init(value: Int) {
        self.value = value
    }
}

// we use the '@' symbol to reference a resource type
let a: @SomeResource <- create SomeResource(value: 0)

// also in functions declarations
pub fun use(resource: @SomeResource) {
    destroy resource
}
```

Any additional uses for the @ symbol? File a PR!

## = (equals)

A simple assignment

```cadence
let a = 1; // assigns a value to a 
```


Any additional uses for the = symbol? File a PR!

## : (colon)

If a colon follows a variable/constant/function declaration, it is used to declare its type.

```cadence
let a: Bool = true; // assigns a value to a 

// or

fun addOne(x: Int): Int { // return type of Int
    return x + 1
}
```

It can also be used in ternary operations to represent the "otherwise" section, such as the following:


```cadence
let a = 1 > 2 ? 3 : 4
// should be read as: 
//   "is 1 greater than 2?"
//   "if YES, then set a = 3,
//   "if NO, then set a = 4.
```


Any additional uses for the : symbol? File a PR!

## _ (underscore)

In Cadence, the _ (underscore) has several uses.

It can be used in variable names, or to split up numerical components.

Examples:

```cadence
let _a = true; // used as a variable name 

// or

let b = 100_000_000 // used to split up a number (supports all number types, e.g. 0b10_11_01)
```

It can also be used to omit the function argument label. Usually argument labels precede the parameter name. The special argument label _ indicates that a function call can omit the argument label. [More info](https://docs.onflow.org/cadence/language/functions/#function-declarations) 

```cadence
// The special argument label _ is specified for the parameter,
// so no argument label has to be provided in a function call.
//
fun double(_ x: Int): Int {
    return x * 2
}
```


Any additional uses for the _ symbol? File a PR! 

## <- (lower than, hyphen) (move operator)

The move operator <- replaces the assignment operator = in assignments that involve resources. To make assignment of resources explicit, the move operator <- must be used when:

- the resource is the initial value of a constant or variable,
- the resource is moved to a different variable in an assignment,
- the resource is moved to a function as an argument
- the resource is returned from a function.

This is because in Cadence resources are linear types which can only exist in a single place at a time. So the move operator figuratively helps show that that resource is being moved and will no longer be available in its previous location/state once it is moved.

[More info](https://docs.onflow.org/cadence/language/resources/#the-move-operator--)

```cadence
resource R {}

let a <- create R() // we instantiate a new resource and move it into a
```

Keep in mind that any time resources are involved, the move (or swap) operator must be used, including in Arrays and Dictionaries! [More info](https://docs.onflow.org/cadence/language/resources/#resources-in-arrays-and-dictionaries)

```cadence
resource R {}

let a <- [
  <- create R(), // we create a new resource R and move it into the Array
  <- create R()  // another time
]
```

Any additional uses for the <- symbol? File a PR!

## <-! (lower than, hyphen, exclamation mark) (Force-assignment operator)

Assigns a resource value to an optional variable if the variable is nil (if it is not nil, it aborts)

This is only used for resources, as they use the exclamation mark.

```cadence
pub resource R {}

var a: @R? <- nil
a <-! create R()
```

## <-> (lower than, hyphen, greater than) (swap operator)

<-> is referred to as the Swap operatior. It swaps values between the variables to the left and right of it. [More info](https://docs.onflow.org/cadence/language/operators/#swapping)

```cadence
let a = 1;  
let b = 2;

a <-> b;
// a = 2
// b = 1
```


Any additional uses for the <-> symbol? File a PR! 

## + (plus), - (minus), * (asterisk), % (percentage sign)

These are all typical arithmetic operators. 

- Addition: +
- Subtraction: -
- Multiplication: *
- Remainder: %

[More info](https://docs.onflow.org/cadence/language/operators/#arithmetic)

## / (forward slash)

The / (forward slash) symbol can be used in two ways:

1. As a division operator `let a = 1/2`

```cadence
let a: Fix64= 2.0
let b: Fix64= 3.0
let c = a / b // = 0.66666666
```
[More info](https://docs.onflow.org/cadence/language/values-and-types/#fixed-point-numbers) on division and Fixed point numbers in Cadence

2. As a [Path](https://docs.onflow.org/cadence/language/accounts/#paths)

```cadence
let storagePath = /storage/path
storagePath.toString()  // is "/storage/path"
```

[More info](https://docs.onflow.org/cadence/language/accounts/#paths) on Paths

## & (ampersand)

The & (ampersand) symbol has several uses. 

The more Cadence-specific use is that of a Reference. In Cadence it is possible to create references to objects, i.e. resources or structures. A reference can be used to access fields and call functions on the referenced object. 

References are created by using the & operator, followed by the object, the as keyword, and the type through which they should be accessed. The given type must be a supertype of the referenced object's type.

[More info](https://docs.onflow.org/cadence/language/references/)

```cadence
let a: String = "hello"
let refOfA: &String = &a as &String
```

References may be authorized or unauthorized.

Authorized references have the auth modifier, i.e. the full syntax is auth &T, whereas unauthorized references do not have a modifier. Lastly, References are ephemeral, i.e they cannot be stored. [More info](https://docs.onflow.org/cadence/language/references/)

```cadence
let a: String = "hello"
let refOfA: &String = &a as auth &String
```

It can be also used as a logical operator (AND), by appearing twice in a row, similar to the double pipe symbol (|| which represents OR):

```cadence
let a = true
let b = false

let c = a && b // false
```


## ? (question mark)

The ? (question mark) symbol has several uses. If a ? follows a variable/constant, it represents an optional. An optional can either have a value or *nothing at all*. 

```cadence
// Declare a constant which has an optional integer type
//
let a: Int? = nil
```

When you see as?, that's a conditional downcasting operator. It can be used to downcast a value to a type. This operator returns an optional, and if the value has a type that is a subtype it will return the value as that type, otherwise it will return `nil`. [More info](https://docs.onflow.org/cadence/language/values-and-types/#conditional-downcasting-operator)

```cadence
// a simple interface that expects a property count
resource interface HasCount {
  count : Int
}

// a Counter resource that conforms to HasCount
resource Counter: HasCount {
      pub var count: Int

    pub init(count: Int) {
        self.count = count
    }
}

// set a reference countRef to &counter with the hasCount interface 
// this is important because ONLY methods in HasCount will be available!
let countRef: &{HasCount} = &counter as &{HasCount}

// BUT, we could also optionally downcast it to Counter
let authCountRef: auth &{HasCount} = &counter as auth &{HasCount}
let countRef2: &Counter = authCountRef as? &Counter
```

It is a big topic, so best to [read the documentation on it](https://docs.onflow.org/cadence/language/values-and-types/#optionals)


It can also be used in ternary operations to represent the "otherwise" section, such as the following:


```cadence
let a = 1 > 2 ? 3 : 4
// should be read as: 
//   "is 1 greater than 2?"
//   "if YES, then set a = 3,
//   "if NO, then set a = 4.
```

It can also be used as a nil-coalescing operator. The nil-coalescing operator `??` returns the value inside an optional if it contains a value, or returns an alternative value if the optional has no value, i.e., the optional value is nil. The nil-coalescing operator can only be applied to values which have an optional type.

```cadence
// Declare a constant which has an optional integer type
//
let a: Int? = nil

// Declare a constant with a non-optional integer type,
// which is initialized to `a` if it is non-nil, or 42 otherwise.
//
let b: Int = a ?? 42
// `b` is 42, as `a` is nil


// Invalid: nil-coalescing operator is applied to a value which has a non-optional type
// (the integer literal is of type `Int`).
//
let c = 1 ?? 2
```

## ! (exclamation mark)

The exclamation mark can be used in several ways:

When it immediately **precedes** a boolean-type variable, it negates it.

```cadence
let a: Bool = true;
let b: Bool = !a;

// b is false
```

When it immediately **succeeds** an *optional* variable, it force-unwraps it. Force-unwrapping returns the value inside an optional if it contains a value, or panics and aborts the execution if the optional has no value, i.e., the optional value is nil.

```cadence
let a: Int? = nil;
let b: Int? = 3; 

let c: Int = a! // panics, because = nil
let d: Int = b! // initialized correctly as 3
```

