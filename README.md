# Rust Memory Safety Demos

Practical examples of Rust's ownership model and common security-related patterns. This repository serves as a reference for understanding how Rust prevents memory safety issues at compile time.

## 🛡️ Why Memory Safety Matters
Memory safety vulnerabilities account for a significant portion of security issues in low-level code. Rust's borrow checker eliminates entire classes of vulnerabilities:
- Use-after-free
- Double-free
- Buffer overflows
- Data races

## 📚 Examples Included

### 1. Ownership Basics
- Moving vs. copying semantics
- Function ownership transfers

### 2. Borrowing & References
- Immutable vs mutable references
- The "one mutable reference" rule

### 3. Common Pitfalls & Solutions
- Dangling references
- Iterator invalidation
- Safe vs unsafe blocks

### 4. Smart Pointers
- `Box`, `Rc`, `Arc` for different ownership patterns

## 🚀 Getting Started
```bash
# Clone and run examples
git clone https://github.com/YOUR_USERNAME/rust-memory-safety-demos.git
cd rust-memory-safety-demos

# Run all examples
cargo run --example ownership_basics
cargo run --example borrowing

🎯 For Solana/Security Context
Understanding these concepts is crucial for:

Writing safe Solana programs

Avoiding common Rust security anti-patterns

Auditing Rust-based smart contracts

📖 Learning Path
Start with ownership_basics.rs

Move to borrowing.rs

Explore lifetimes.rs

Study unsafe_guidelines.rs

Part of my preparation for blockchain security development. Each example includes comments explaining the security implications.


**File: `src/examples/ownership_basics.rs`**

```rust
//! Example 1: Ownership Basics
//! Demonstrates how Rust's ownership model prevents memory safety issues

fn main() {
    println!("=== Ownership Basics ===\n");
    
    // 1. Moving ownership (not copying)
    let s1 = String::from("hello");
    let s2 = s1; // s1 is moved to s2, s1 is no longer valid
    
    // println!("{}", s1); // This would compile error: borrow of moved value
    println!("s2: {}", s2); // This works
    
    // 2. Function ownership transfer
    takes_ownership(s2); // s2 moves into the function
    // println!("{}", s2); // Error: s2 is no longer valid here
    
    // 3. Returning ownership
    let s3 = gives_ownership();
    println!("s3 from function: {}", s3);
    
    // 4. Clone when you need actual copy
    let s4 = String::from("clone example");
    let s5 = s4.clone(); // Expensive but valid
    println!("s4: {}, s5: {}", s4, s5);
    
    println!("\n✅ Ownership prevents:");
    println!("   - Use-after-free");
    println!("   - Double-free errors");
    println!("   - Memory leaks (when combined with RAII)");
}

fn takes_ownership(some_string: String) {
    println!("Function owns: {}", some_string);
} // some_string goes out of scope and memory is freed

fn gives_ownership() -> String {
    let some_string = String::from("returned string");
    some_string // Ownership is moved out to caller
}
