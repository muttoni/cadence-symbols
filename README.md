# Cadence Symbols Cheatsheet

A simple TLDR reference containing all of Cadence's symbols and what they do. 

Just <kbd>CTRL</kbd>/<kbd>CMD</kbd> + <kbd>F</kbd> and type in the symbol you want to look up.

Please feel free to send a PR with more/better examples and symbols.

## @ (at) 

The @ symbol is used to denote whether a specific type is a resource, and must therefore adhere to the Resource-specific lifecycle in Cadence (create, destroy, move). [More info](https://docs.onflow.org/cadence/language/resources/)

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

## = (equals)

A simple assignment

```cadence
let a = 1; // assigns a value to a 
```

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

## <-> (lower than, hyphen, greater than) (swap operator)

<-> is referred to as the Swap operatior. It swaps values between the variables to the left and right of it. [More info](https://docs.onflow.org/cadence/language/operators/#swapping)

```cadence
let a = 1;  
let b = 2;

a <-> b;
// a = 2
// b = 1
```

## + (plus), - (minus), * (asterisk), / (forward slash), % (percentage sign)

These are all typical arithmetic operators. 

- Addition: +
- Subtraction: -
- Multiplication: *
- Division: /
- Remainder: %

[More info](https://docs.onflow.org/cadence/language/operators/#arithmetic)

## & (ampersand)

The & (ampersand) symbol has several uses. 

It can be used as a logical operator (AND), by appearing twice in a row:


```cadence
let a = true
let b = false

let c = a && b // false
```

It can also be used to 


## ? (question mark)

The ? (question mark) symbol has several uses. If a ? follows a variable/constant, it represents an optional. An optional can either have a value or *nothing at all*. This is important so we can simplify 

It is a big topic, so best to [read the documentation on it](https://docs.onflow.org/cadence/language/values-and-types/#optionals)


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


If can also be used as a nil-coalescing operator. The nil-coalescing operator `??` returns the value inside an optional if it contains a value, or returns an alternative value if the optional has no value, i.e., the optional value is nil. The nil-coalescing operator can only be applied to values which have an optional type.

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

When it immediately **precedes** a variable, it negates it.

```cadence
let a = false;
let b = !a;
```

When it immediately **succeeds** an *optional* variable, it force-unwraps it. Force-unwrapping returns the value inside an optional if it contains a value, or panics and aborts the execution if the optional has no value, i.e., the optional value is nil.

```cadence
let a: Int? = nil;
let b: Int? = 3; 

let c: Int = a! // panics, because = nil
let d: Int = b! // initialized correctly as 3
```

