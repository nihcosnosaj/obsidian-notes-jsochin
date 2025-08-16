ITER Hello, World! In Rust:
```
fn main() {
    println!("Hello, World!");
}

```

To compile and run the executable:

```
$ rustc hello.rs
$ ./hello

```

A few things about Rust:
- curly-braces
- semicolons
- C++ style comments (`//`)
- `main` function
- The exclamation mark `!` indicates the call is a macro call

Try to see the compiler as a strict but friendly helper!

We can declare a variable with `let`

`let number = 42`

The macro `println!` takes a format string, much like in python, and some values to place into the brackets.

Also to note: `assert_eq!`

```
fn main() {
    let answer = 42;
    assert_eq!(answer,42);
}
```


Loops and Ifs

```
fn main() {
	for i in 0..5 {
		println!("Hello {}", i);
	}
}
```

We can do some cool things with if/else loops as well:
```
fn main() {
    for i in 0..5 {
        if i % 2 == 0 {
            println!("even {}", i);
        } else {
            println!("odd {}", i);
        }
    }
}

```

Heres the same thing as above in a more interesting way:
```
fn main() {
    for i in 0..5 {
        let even_odd = if i % 2 == 0 {"even"} else {"odd"};
        println!("{} {}", even_odd, i);
    }
}

```


## Arithmetic

Lets add all the numbers from 0 to 4:
```
fn main() {
    let mut sum = 0;
    for i in 0..5 {
        sum += i;
    }
    println!("sum is {}", sum);
}

```

This won't compile! `let` variables can only be assigned value once when declared. If you want to make them mutable, add `mut`

Why are these rust variables read-only by default? --> It's hard to track where writes happen in larger scripts, so rust likes to make mutability explicit

Rust is both statically-typed and strongly-typed.

Each operator (like `+=`) corresponds to a ***trait*** , basically an abstract interface that must be implemented for each concrete type. 

`AddAssign` is the name of the trait implementing the `+=` operator

Rust likes to be explicit!! It will not silently convert integers to floats for you! If we changed `sum` in the above function to be a float value of 0.0, we'd have to cast that value as a floating point explicitly:
```
fn main() {
	let mut sum = 0.0;
	for in in 0..5 {
		sum += i as f64;	
	}
	println!("sum is {}", sum);
}

```


## Function Types are Explicit!

Functions are one place where the compiler will NOT work out the types for you. Rust requires explicit type signatures for functions.

```
fn sqr(x: f64) -> f64 {
    return x * x;
}

fn main() {
    let res = sqr(2.0);
    println!("square is {}", res);
}

```

Functions in Rust don't really use a return statement.
The body of the function has a value of its last expression, so returns aren't really needed as they are in python. 

The () type is the empty type.

Lets make a little no-return function to ensure a number always falls in the given range:
```
fn clamp(x: f64, x1: f64, x2: f64) -> f64 {
    if x < x1 {
        x1
    } else if x > x2 {
        x2
    } else {
        x
    }
}

```

Just to note, using `return` isn't wrong, but code is just cleaner without it. If you ever want to return early from a function, you can still use return.

Some operations can be expressed recursively in a very elegant way:

```
fn factorial(n: u64) -> u64 {
	if n==0 {
		1
	} else {
		n * factorial(n-1)
	}
}


```


Values can also be passed by a reference. References are created by `&` and dereferenced by `*`

```
fn by_ref(x: &i32) -> i32 {
	*x + 1
}

fn main() {
	let i = 10;
	let res1 = by_ref(&i);
	let res2 = by_ref(&41);
	println!("{} {}", res1, res2);
}


```

And if you want a function to modify one of its arguments? Mutable references!

```
fn modifies(x: &mut f64) {
	*x = 1.0;
}

fn main() {
	let mut res = 0.0;
	modifies(&mut res);
	println!("res is {}", res);
}

```

You have to explicitly pass the reference (with `&`) and explicitly dereference with `*`

Basically, Rust is pushing you towards returning values from functions directly. Rust does have powerful ops to express "this operation succeeded and here is the result" so `&mut` isn't needed so often. 

