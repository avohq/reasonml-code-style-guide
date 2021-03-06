# The Style Guide

1. Define meaningful types for every variant and polymorphic variant you introduce. For example, when defining `MergeBranch` you would create `type branchName = string;` and the definition would look like: `MergeBranch(branchName)` instead of `MergeBranch(string)`
2. Define type signatures explicitly unless you're sure that they should not be defined. Even when the compiler can infer the type it still is often useful to state it explicitly for readability of the code, especially outside of the IDE.
3. Use named parameters if a function has multiple parameters of same type. This will prevent accidentally mixing up the parameters when calling the function.
4. Tend to use named parameters more often. This is advised for readability.
5. Imperative code (i.e. the `ref` keyword) should be used only with a very strong reason.
6. Default to using non polymorphic variants (```type rgb = Red | Green | Blue;``` over ```type rgb = [`Red | `Green | `Blue];```). We think of polymorphic variants as a lesser-typed approach and prefer to have stronger typing.
7. Prefer using uncurried functions over curried (i. e. *`reduceU` over `reduce`* in Belt.List*)*, it will help the compiler to produce cleaner error messages in cases when there is a parameters mismatch, which happens quite often during refactoring. Another benefit is that it works faster.
8. All new components should be Jsx3 components. This is done to be able to use the latest language features.
9. Put multiline styles in the top of the file. 
10. When deciding between `thing->function->function` and `function(thing)->function` read it out and pick the one that makes more sense in a natural language, i.e. `Firebase.firestore(firebase)->Firebase.root("main")` and `maybeSomething->Belt.Option.getWithDefault("definitelySomething")`
11. Avoid switch-like funs 

    ```jsx
    ~~let map = f =>
      fun
      | Some(v) => Some(f(v))
      | None => None;~~
    ```
  This syntax is hard to parse and it is deprecated in latest versions of the language.
12. When working with JSON create decoders and encoders with the `bs-json` lib
    ```bash
    Json.Decode.(field("token", string, "myToken"));

    Json.Encode.(object_([
                  ("token", string("myToken")),
                  ("returnSecureToken", bool(true)),
                ]);
    ```
    see [more about using variants](https://stackoverflow.com/questions/50908342/convert-json-field-to-reasonml-variant) and [this blog post](https://itnext.io/decoding-nested-json-objects-in-reasonml-with-bs-json-4cab75fbe308) for more examples.

    We tried other approaches but they all tend to end up with similar amount of work and less constrol.
13. When defining functions keep the types definition in the function, not in the variable, to allow more freedom for type inferrence, i.e. `let a = (b: string, c: int): string => b === "ok"` over `let a: ((string, int) => boolean) = (b, c) => b === "ok"`
14. **ALWAYS** use `Js.Array2` and `Js.String2`. **NEVER** use `Js.Array` and `Js.String`. This is required to keep the `->` piping consistent. A nice suggestion of how to achieve that can be found here https://github.com/avohq/reasonml-code-style-guide/issues/2
15. Use `List`s for resursive data structures. Use `Array` otherwise. I.e.
  ```
  let rec len = (myList: list('a)) =>
    switch myList {
      | [] => 0
      | [_, ...tail] => 1 + len(tail)
    };
  ```
16. Do not overuse reduce. Consider other options, like `map` and `flatMap` to achieve the result. Prefer reduce over `ref` though.
17. Open `Belt` globally. It saves a lot of typing.
18. Prefer `Belt.Result` over throwing exceptions. This would make the execution flow more homogeneous. Exceptions are generally considered to be avoided nowadays.

