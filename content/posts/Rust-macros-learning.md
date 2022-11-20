---
title: "Rust Macros Learning"
date: "2022-04-16T19:23:26+08:00"
# draft: true
---

# The Rust macro learning: A Methodical Introduction
## [Macros in the AST](https://veykril.github.io/tlborm/syntax-extensions/ast.html#macros-in-the-ast)
There are four types of macros in rust.
## macro_rules!
As noted previously, `macro_rules!` is *itself* a syntax extension, meaning it is *technically* not part of the Rust Syntax. It uses the following forms:
```rust
macro_rules! $name {
    $rule0 ;
    $rule1 ;
    // ...
    $ruleN ;
}
```
There must be *at least* one rule, and you can omit the semicolon after the last rule. You can use brackets`[]`, parentheses`()` or braces`{}`.

Each "rule" looks like the following:
```
    ($matcher) => {$expansion}
```
Like before, the types of parentheses used can be any kind, but parenthese around the matcher and braces around the expansion are somewhat conventional. The expansion part of a rule is also called its **transcriber**.

Note that the choice of the parentheses does not matter in regards to how the mbe(macro by example) macro may be invoked. In fact, function-like macros can be invoked with any kind of parentheses as well, but invocations with `{ .. }` and `{ ... };`, notice the trailing semicolon, are special in that their expansion will **always** be parsed as an **item**.

## Metavariables
Matchers can also contain captures. These allow input to be matched based on some general grammer category, with the result captured to a metavariable which can then be substituted into the output.

Captures are written as a dollar `$` followed by an identifier, a colon `:`, and finally the kind of capture which is also called the fragment-specifier, which must be one of the following:
* `block`: a block (i.e. a block of statements and/or an expression, surrounded by braces)
* `expr`: an expression
* `ident`: an identifier (this includes keywords)
* `item`: an item, like a function, struct, module, impl, etc.
* `lifetime`: a lifetime (e.g. `'foo`, `'static`, ...)
* `literal`: a literal (e.g. `"Hello World!"`, `3.14`, ':crab:', ...)
* `meta`: a meta item; teh things that go inside the `#[...]` and `#![...]` attributes
* `pat`: a pattern
* `path`: a path (e.g. `foo`, `::std::mem::replace`, `transmute::<_, int>`, ...)
* `stmt`: a statment
* `tt`: a single token free
* `ty`: a type
* `vis`: a possible empty visibility qualifier (e.g. `pub`, `pub(in crate)`, ...)

For example, here is a `macro_rules!` macro which captures its input as an expression under the metavariable `$e`:
```rust
macro_rules! one_expression {
    ($e:expr) => {...};
}
```
These metavariables leverage the Rust compiler's parser, ensuring that they are always "correct". An `expr` metavariable will *always* capture a complete, valid expression for the version of Rust being compiled.

To refer to a metavariable you simply write `$name`, as the type of the variable is already specified in the matcher. For example:
```rust
macro_rules! time_five {
    ($e:expr) => { 5 * $e };
}
```
Much like macro expansion, metavariables are substituted as complete AST nodes. This means that no matter what sequence of tokens is captured by `$e`, it will be interpreted as a single, complete expression.

You can also have multiple metavariables in a single matcher: 
```rust
macro_rules! multiply_add {
    ($a:expr, $b:expr, $c:expr) => { $a * ($b + $c) }; 
}
```
Add use them as often as you like in the expansion:
```rust
marco_rules! discard {
    ($e:expr) => {};
}
marco_rules! repeat {
    ($e:expr) => { $e; $e; $e; };
}
```
There is also a special metavariable called `$crate` which can be used to refer to the current crate.

## Repetitions 
Matchers can contain repetitions. These allow a sequence of tokens to be matched. These have the general form `$ ( ... ) sep rep`.
* `$` is a literal dollar token.
* `( ... )` is the paren-grouped matcher being repeated.
* `sep` is an *optional* separator token. It may not be a delimiter or one of the repetition operators. Common examples are `,` and `;`.
* `rep` is the *required* repeat operator. Currently, this can be: 
    * `?`: indicating at most one repetition
    * `*`: indicating zero or more repetitions 
    * `+`: indicating one or more repetitions
    
    Since `?` represents at most one occurence, it cannot be used with a separator.
    
Repetitions can contain any other valid matcher, including literal token trees, metavariables, and other repetitions allowing arbitrary nesting.

Repetitions use the same syntax in the expansion and repeated metavariables can only be accessed inside of repetitions in the expansion.

For example, below is a mbe macro which formats each element as a string. It matches zero or more comma-separated expressions and expands to an expression that constructs a vector.
```rust
macro_rules! vec_strs {
    (
        // Start a repetition:
        $(
            // Each repeat must contain an expression...
            $element:expr
        )
        // ...separated by commas...
        ,
        // ...zero or more times.
        *
    ) => {
        // Enclose the expansion in a block so that we can use 
        // multiple statements.
        {
            let mut v = Vec::new();

            // Start a repetition
            $(
                // Each repeat will contain the following statement, with
                // $element replaced with the corresponding expression
                v.push(format!("{}", $element));
            )*

            v
        }
    };
}

fn main() {
    let s = vec_strs![1, "a", true, 3.14159f32];
    assert_eq!(s, &["1", "a", "true", "3.14159"]);
}
```
You can repeat multiple metavariables in a single repetition as long as all metavariables repeat equally often. So this invocation of the following macro works:
```rust
macro_rules! repeat_two {
    ($($i:ident)*, $($i2:ident)*) => {
        $( let $i: (); let $i2: (); )*
    }
}

repeat_two!( a b c d e f, u v w x y z );
```
But this does not:
```rust
...

repeat_two!( a b c d e f, x, y, z );
```
failing with the error 
```
error: meta-variable `i` repeats 6 times, but `i2` repeats 3 times
 --> src/main.rs:6:10
  |
6 |         $( let $i: (); let $i2: (); )*
  |          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
