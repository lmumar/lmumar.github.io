---
layout: post
title: Rust - Weird trick #1
---

### Rust - Weird trick #1

In Rust you cannot create another reference to a value if you already have a mutable reference to it. Example:
```
fn main() {
    let mut x = 10;
    let rx = &mut x;
    *rx = 100;
    
    let irx = &x;
    println!("irx={}", irx);
}
```
If you compile this code you will receive the following errors:
```
   Compiling playground v0.0.1 (file:///playground)
error[E0502]: cannot borrow `x` as immutable because it is also borrowed as mutable
 --> src/main.rs:6:16
  |
3 |     let rx = &mut x;
  |                   - mutable borrow occurs here
...
6 |     let irx = &x;
  |                ^ immutable borrow occurs here
...
9 | }
  | - mutable borrow ends here

error: aborting due to previous error

error: Could not compile `playground`.

To learn more, run the command again with --verbose.
```

However you can trick the borrower checker by using a reference to a reference instead. So the code below will compile  and run successfully in rust.
```
fn main() {
    let mut x = 10;
    let rx = &mut x;
    *rx = 100;
    
    let irx = &rx;
    println!("irx={}", irx);
}
```

output:
```
Standard Error

   Compiling playground v0.0.1 (file:///playground)
    Finished release [optimized] target(s) in 0.54 secs
     Running `target/release/playground`

Standard Output

irx=100
```
