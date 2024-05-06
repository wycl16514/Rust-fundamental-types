Rust is quit like c/c++ in that it has pointer. Lot's of languages like java, js, python have removed pointer because they have virtual machine for management of memory. Rust has its own memory management mechanism，
but still provide pointer for efficiency reason. Pointer is something like second order derivation, you need to tweak your mind to understand it.

Pointer is actually a number value used to indicate the memory address for its target. In Rust pointer turn out into three categories: reference, boxes, and unsafe pointers.

We have seen reference before, any type prefix with '&' is a reference type such as &String, &i32, for the code like following:
```
let a :i32 = 10;
let b = &a;
```

If you machine is 32 bits, then b is a 32 bits integer or 64 bits integer if your machine is 64 bits that represent the memory address where the value of variable a is saving. There are two kinds of reference:
&T immutable reference which means you can only read its value but can't write to. The other is mut &T which is mutable reference which means you can read and write to its value, let's see example code:
```r
fn main() {
    /*
    reference means right to use but not the right to own
    */
    //let a: i32 = 10;
    //illegal to use mutable reference to immutable variable.
    //let b = &mut a;
    let mut c: i32 = 10;
    let e = &c;
    //allow for immutable reference to mutable variable
    println!("value of e: {}", e);
    //allow for mutable reference to mutable variable
    let d = &mut c;
    d.add_assign(1);

    //arithmetic operation can apply to variable but not to the reference
    // d = d + 1;
    //&T is just like add a box for type T and * is the reverse operation
    //just like take the object out of the box
    *d = *d + 1;
    println!("value of c is : {}", c);
}
```
As you can see above code, we can't use a mutable reference to an immutable variable, but we can use mutable or immutable reference to a mutable variable.

The other kind of pointer type is boxes, it just like duplicate a patch of memory, we can see the following code:
```r
    //boxes
    let t = (12, "apple");
    let b = Box::new(t);
    //{:p} print its address
    println!("content of t: {:?}", t);
    println!("content of b: {:?}", b);
    println!("address of t is :{:p}", &t);
    println!("address of b is: {:p}", &b);
    /*
    we have to patches of memory with the same content (12, "apples")
    */
```
Running code above gives the following result in my machine:
```r
content of t: (12, "apple")
content of b: (12, "apple")
address of t is :0x7ff7b5d9f1f0
address of b is: 0x7ff7b5d9f208
```
If you don't know what memory address it is, try think about you apply driver license in Us and in France, both licenses are for the same people(content) but they have different license number(memory address).

The third kind of pointer is called raw pointer, it is just like pointer in c/c++, it is very unsafe to manipulate with raw pointer, you will have bugs like c/c++ related to pointer in rust 
if you are using raw pointer, we will goto raw pointer in later section.

