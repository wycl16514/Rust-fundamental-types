We have been through several rust projects and have our hands wet, now we can dive deep into the details of rust language and solve problems we have in previous 
proejcts. In this section let's check types in Rust.

There are many basic types in Rust, for numbers they are i8,...i128 for signed number, and u8...u128 for unsign number. f32, f64 for float number, and isize and usize
are signed and unsign number with bit width as your machine, if you are using 32 bits CPU, then they are 32 bits, if you are using 64 bits cpu, then they are 64bits.
Rust requires that we should use usize for index of array, and the counts or size of array or vector is using usize value too.

We don't need to bog down on basic type details, we can search for more detail by google when it is needed. One future of Rust need  to metion is its type inference.
That is we don't need to state the type in the code, rust compiler can inference the type for us which can greatly same our typing, let's see and example:
```r
fn print_type_of<T>(_: &T) {
    println!("{}", std::any::type_name::<T>())
}

fn build_vector_i16() -> Vec<i16> {
    let mut v = Vec::new();
    v.push(10);
    print_type_of(&v[0]);
    print_type_of(&v);
    v
}

fn build_vector_u16() -> Vec<u16> {
    let mut v = Vec::new();
    v.push(10);
    print_type_of(&v[0]);
    print_type_of(&v);
    v
}

fn main() {
    build_vector_i16();

    build_vector_u16();
}

```
In the code above, we have a quit advance function that is print_type_of, its a type template function, we will go to it in later section, we just now it can print
out the type of its input, pay attion to build_vector_i16 and build_vector_u16, their return types are Vec<i16> and Vec<u16>, and we don't need to indicate the type
of vector in the body of the function, because rust compiler can reference its type by looking at the return type of the function and inference out the returned 
vector type, and we can see that when we push value into the vector, rust compiler can convert the value of 10 into i16 and u16 respectively.

If there is not type inference we need to state the type for vector and pushed value like following:
```r
let mut v = Vec<i16>::new();
v.push(10i16);

let mut v = Vec<u16>::new()
v.push(10u16);
```

you can see we can save quit some typing. Running the code above we get the following result:
```r
i16
alloc::vec::Vec<i16>
u16
alloc::vec::Vec<u16>
```

In Rust, character like 'a', 'b' has their own type and is different from u8, but we can convert char to u8 type like this b'a', for example, the following code:
```r
    let c = b'x';
    print_type_of(&c);
```
it will print out u8 as result. We can convert one integer type into other type by using key word "as" like following:
```r
   let d = 62i16 as u32;
    print_type_of(&d);
```
Running the above code will have result as u32. But we need to careful about overflow for type convertion like following:
```r
    let e = 1000i16 as u8;
    println!("value of e :{}", e);
```
The value of e turns out to 232 which is smaller than 1000 because of arithmetic overflow. Even the basic types are class that we cann call method on them directly:
```r
println!("2u16.pow(4) is {}", 2u16.pow(4));
```
But when calling methond on integer type value, you need to state its type otherwise the compiler will complain:
```r
 println!("2.pow(4) is {}", 2.pow(4));
```
Above code will have the following error:
```r
error[E0689]: can't call method `pow` on ambiguous numeric type `{integer}`
  --> src/main.rs:41:34
   |
41 |     println!("2.pow(4) is {}", 2.pow(4));
   |                                  ^^^
   |
help: you must specify a concrete type for this numeric value, like `i32`
   |
41 |     println!("2.pow(4) is {}", 2_i32.pow(4));
```
When arithmetic operation on given type causes overflow, rust will panic in debug mode and wrap around the value automaticlly, the following code will panic in debug
mode:
```
 let mut i = 1;
    loop {
        i *= 10;
    }
```
Running the code will result in following:
```r
thread 'main' panicked at 'attempt to multiply with overflow', src/main.rs:46:9
```
If we don't want the wrap around in release build and panic when overlow happended, we can do the following:
```
/*
    check operation on integer type return Option, if the operation has not
    overfolow, it returns Some(v) otherwise returns Err(e)
    */
    let mut i: i32 = 1;
    match i.checked_add(30) {
        Some(v) => {
            println!("add result is :{}", v)
        }
        None => {
            println!("add error: {}", e)
        }
    }
```
If you wish the operation can continue and wrapping around by rust compiler and you when to know whether there is overflowing , then you can use overflow operation
like following:
```r
 /*
    enable wrap around when overflow and know whether overflow happended
    */
    let mut res = 255u8.overflowing_sub(2);
    println!("sub result: {} is overflowing?: {}", res.0, res.1);

    res = 255u8.overflowing_add(2);
    println!("add result: {} is overflowing?: {}", res.0, res.1);
```
Running the above code will have the following result:
```r
sub result: 253 is overflowing?: false
add result: 1 is overflowing?: true
```
