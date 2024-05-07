String in Rust is a little bit confusing, it has two kinds of string, one is for string literal and its type is &str, the other is string variable with type String. If you are familiar with c++, there are two 
kinds of string too, one is const char*, the other is from standard library std::string. You can't change the content of string literal, you can only read to it like content of string, length of the string.
For string variable, you can write to it like change its content.

String literal in Rust is contained in double quotes, and you can have newlines in the string, and the newline will shown when you print out the string, but you can ignore the newline if you place a backslash
before it, let's check its effect by following code:
```r
fn main() {
    //string literal with newlines
    println!(
        "Hello world!
    and have a nice day",
    );
    //newline will ignore by backslash
    println!(
        "Hello world! \
    and have a nice day"
    );
}
```
Run the code above you will get the following result:
```r
Hello world!
    and have a nice day
Hello world! and have a nice day
```
You can see the first one span two lines and the second has only one line. If you want to print out special character like backslash, you need to escape it with another backslash, but if you use raw string,
that is the string literal begin with a r, then all special character in the string will escape automatically, like following:

```r
//escape special character
    println!("c:\\Program Files \\Rust");
    //raw string escape automatically
    println!(r"c:\Program Files\Rust");
    println!(r"It's my word");
```

We will use raw string a lot when you are construct regular expression. One problem for raw string is you can't put double quote inside it, when compiler see any double quote after the first one, it will think
that's the end of raw string, like following are illegal:
```r
println!(r"hello" world!");
```
If you want to include double quote in the string  ,you can do the following:
```r
 println!(
        r###"
    we can include any quotes like " or ' and any special characters
    like \ in such raw string.
    "###
    );
```

We can convert a string into the form of byte array like following:
```
 //convert string into byte slice
    let method = b"GET";
    for char in method {
        println!("value of char is : {}", char);
    }
```
Running the above code will get the following result:
```r
value of char is : 71
value of char is : 69
value of char is : 84
```
That is we turn each character in "GET" into byte values and the ASCII value for ‘G’ is 71, and ASCII value ofr 'E' is 69. And we can turn raw string into byte array like following:
```r
 let raw = br###"" ' \ #"###;
    for char in raw {
        println!("value of char in raw string is : {}", char);
    }
```

Another kind of string is String variable. All ASCII character will use one byte and other character will use UTF-8 encoding for variable byte length, let's check it by code:
```r
 //String encode
    let strs = "hi你好".to_string();
    println!("length of string: {}", strs.len());
    let slice = &strs[2..];
    println!("string slice is : {:?}", slice);
    let char_1 = &strs[2..5];
    let char_2 = &strs[5..];
    println!("char_1: {:?}, char_2: {:?}", char_1, char_2);
```
The code above gives the following result:
```r
length of string: 8
string slice is : "你好"
char_1: "你", char_2: "好"
```
you can see each chinese character has three bytes. There are some useful ways to create String , like following:
```r
//formatting string
    let format_str = format!("{:02}:{:02}", 123, 234);
    println!("format string: {}", format_str);
    //concat from string vect
    let vec_strs = ["hello", "world"];
    println!("concat string vec:{}", vec_strs.concat());
    println!("join string vec:{}", vec_strs.join(", "));
```
The above code gives following result:
```r
format string: 123:234
concat string vec:helloworld
join string vec:hello, world
```
There are many useful operations on string some of them like following:
```r
//useful operations on string
    println!("is containing: {}", "peanut".contains("nut"));
    println!("{}", "hello, world!".replace("world", "you"));
    println!("trimming string:{}", "   clean   ".trim());
    for word in "hello,world,how,are,you".split(",") {
        println!("word of split is :{}", word);
    }
```
The code above has the following result:
```r
is containing: true
hello, you!
trimming string:clean
word of split is :hello
word of split is :world
word of split is :how
word of split is :are
word of split is :you
```
