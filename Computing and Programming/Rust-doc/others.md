  هذا الملف للمعلومات اللي ممكن تتفيد بالمستقبل :

# integer handling (i128 , i32 , u128 , u32) over flow 

signed variant can store numbers from -`(2^n - 1) to 2^n - 1 - 1` , where `n` is the number of bits that variant uses. So an `i8` can store numbers from` -(27) to 27 - 1`, which equals `-128 to 127.`
Unsigned variants can store numbers from 0 to 2^n - 1, so a `u8` can store numbers from `0 to 2^8 - 1, which equals 0 to 255.`

When you’re compiling in release mode with the `--release` flag, Rust does _not_ include checks for integer overflow that cause panics.Instead, if overflow occurs, Rust performs _two’s complement wrapping_. 
	In short, values greater than the maximum value the type can hold “wrap around” to the minimum of the values the type can hold. 
	In the case of a `u8`,the value `256` becomes `0`
	the value `257` becomes `1`, and so on. 
	=
The program won’t panic, but the variable will have a value that probably isn’t what you were expecting it to have. Relying on integer overflow’s wrapping behavior is considered an **error**.


#  looping syntax magic

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break; // break the internal loop 
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

#### [Loop Labels to Disambiguate Between Multiple Loops](file:///home/agent47/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/share/doc/rust/html/book/ch03-05-control-flow.html#loop-labels-to-disambiguate-between-multiple-loops)

If you have loops within loops, `break` and `continue` apply to the innermost loop at that point. You can optionally specify a _loop label_ on a loop that you can then use with `break` or `continue` to specify that those keywords apply to the labeled loop instead of the innermost loop. Loop labels must begin with a single quote. Here’s an example with two nested loops:P

# references and borrowing 
Mutable references have one big restriction: if you have a mutable reference to a value, you can have no other references to that value. This code that attempts to create two mutable references to s will fail:

Rust enforces a similar rule for combining mutable and immutable references. This code results in an error:
```rust
  let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);
```
 - At any given time, you can have _either_ one mutable reference _or_ any number of immutable references.
- References must always be valid.(look dangling references 4.2 rust learning book )

# cargo 

- **Packages:** A Cargo feature that lets you build, test, and share crates
- **Crates:** A tree of modules that produces a library or executable
- **Modules** and **use:** Let you control the organization, scope, and privacy of paths
- **Paths:** A way of naming an item, such as a struct, function, or module

	[cargo.help](file:///home/agent47/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/share/doc/rust/html/book/ch07-02-defining-modules-to-control-scope-and-privacy.html)
	[crate API guide ](https://rust-lang.github.io/api-guidelines/) 

```rust
super:: // goes up one mod samilir to .. in filesystem
crate:: // goes to the root of the crate[main.rs | lib.rc] like / in filesytem 
:: // starts from the current module (relative path)

``` 

# lifetime
