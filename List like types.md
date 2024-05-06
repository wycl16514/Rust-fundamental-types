There is a saying that program = data structure + algorithm, for my own programming experiences, the most used data structures are array or slice and map. I believe most people will agree with me. Rust has good
support for array like data structure, they are array, vector and slice, let's check them one by one.

First Array is fixed size container, you once its size is fixed, you can't make it longer or shorter any more, that's why it won't use very ofen in Rust. Array contains fixed number of objects of the same type.
Let's see how to init arrays in Rust:
```r
fn main() {
    //array is fixed in size of container for objects with the same type
    //it is defined by [T; num], T is for object type and num is the number
    //in the container, you need to put exactly the given number of elements in it
    let numbers: [u32; 6] = [6, 5, 4, 3, 2, 1];
    let countries = ["American", "France", "China"];
    println!("numbers[3] is {}", numbers[3]);
    println!("countries[1] is {}", countries[1]);
    println!("length of numbers is : {}", numbers.len());
    //we can initilize an array with given value
    //an array with 10 0 in it
    let all_zeros = [0u8; 10];
    println!("content of allZeros: {:?}", all_zeros);
    //we can call methods on array or slice, but we need it to be mutable
    let mut chaos = [3, 5, 7, 2, 1, 6];
    chaos.sort();
    println!("chaos after sort: {:?}", chaos);
}

```
Running above code will give following result:
```r
numbers[3] is 3
countries[1] is France
length of numbers is : 6
content of allZeros: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
chaos after sort: [1, 2, 3, 5, 6, 7]
```

Vector is more flexible than array because it is resizable according to your need. Following code are ways to initialize vector and show its usage:
```r
let mut odds = vec![1, 3, 5, 7];
    odds.push(9);
    println!("odds are: {:?}", odds);
    let mut evens: Vec<u32> = Vec::new();
    //add elements
    evens.push(2);
    evens.push(4);
    evens.push(6);
    evens.push(8);
    //remove the second element
    evens.remove(1);
    //insert element at the position of 2
    evens.insert(2, 12);
    println!("events: {:?}", evens);
    //vector share the same methods as array
    evens.reverse();

    println!("events after reverse: {:?}", evens);
    //a fancy but useful way to create a vector
    //need to give the type here
    let v: Vec<i32> = (0..10).collect();
    println!("v is: {:?}", v);
    //pop will remove the last element, if the vector is empty, pop return None
    //otherwise return Some(v)
    let mut strs: Vec<&str> = vec![];
    let mut res = strs.pop();
    println!("pop result for empty vector: {:?}", res);
    strs.push("hello");
    res = strs.pop();
    println!("pop result with vector of one element: {:?}", res);
    //most used operation for vector is looping
    //skip the first elments
    let digits: Vec<i32> = (10..20).skip(1).collect();
    for d in digits {
        println!("digit: {}", d);
    }
```

Finally let's look at slice, it is a reference to part of array or vector, when you want to extract part of array or vector for some purpose, you can use slice to do it easily:
```r
    //take out part of array or vector
    //it is defined by &[T]
    let part_of_array: &[u32] = &numbers[2..4];
    println!("part of numbers: {:?}", part_of_array);
    let mut str_vecs: Vec<&str> = vec!["hello", "world!", "how", "are", "you"];
    let part_of_vecs: &mut [&str] = &mut str_vecs[2..4];
    println!("part of vecs: {:?}", part_of_vecs);
    //change slice will change the vec
    part_of_vecs[0] = "here!";
    println!("vec after slice changed: {:?}", str_vecs);
```
We really need to pay attention to the last line, when we change the slice, we change the original vector or array, the result for running the above code is:
```r
part of numbers: [4, 3]
part of vecs: ["how", "are"]
vec after slice changed: ["hello!", "world!", "here!", "are", "you"]
```
