# vec-reg

Generalized regex like pattern match for vector.

[![Build status](https://github.com/pocket7878/vec-reg/actions/workflows/check.yaml/badge.svg?branch=main)](https://github.com/pocket7878/vec-reg/actions/workflows/check.yml)
[![Crates.io](https://img.shields.io/crates/v/vec-reg)](https://crates.io/crates/vec-reg)
[![Documentation](https://docs.rs/vec-reg/badge.svg)](https://docs.rs/vec-reg)

## Install

```toml
# Cargo.toml
[dependencies]
vec-reg = "0.3.0"
```

## Usage

```rust
use vec_reg::{Regex, vec_reg};

fn build_without_macro() {
  let is_fizz = |x: &i32| x % 3 == 0;
  let is_buzz = |x: &i32| x % 5 == 0;
  let is_fizz_buzz = |x: &i32| x % 15 == 0;
  let reg = Regex::concat(
      Regex::satisfy(is_fizz),
      Regex::repeat1(Regex::concat(Regex::satisfy(is_buzz), Regex::satisfy(is_fizz_buzz))),
  )
  .compile();
  assert!(!reg.is_full_match(&vec![1, 2, 3]));
  assert!(reg.is_full_match(&vec![3, 5, 15]));
  assert!(reg.is_full_match(&vec![6, 10, 15, 10, 30]));
}

fn build_with_macro() {
  let is_fizz = |x: &i32| x % 3 == 0;
  let is_buzz = |x: &i32| x % 5 == 0;
  let reg = vec_reg!([is_fizz]([is_buzz][|x| x % 15 == 0])+).compile();    
  assert!(!reg.is_full_match(&vec![1, 2, 3]));
  assert!(reg.is_full_match(&vec![3, 5, 15]));
  assert!(reg.is_full_match(&vec![6, 10, 15, 10, 30]));
}
```
