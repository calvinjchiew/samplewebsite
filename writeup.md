# CSCI E-51 Final Project Writeup
**The MiniML Language**<br>
Calvin J Chiew, Spring 2017

## Lexical Scoping Extension
As suggested by the project specification, I pursued the implementation of lexical scoping as an extension to the MiniML language. This was achieved by the use of closures, which bind functions to their lexical environment at definition time. During application, functions are evaluated based on their lexical environment, instead of the dynamic environment.

As a result, given the expression

```ocaml
let x = 1 in
let f = fun y -> x + y in
let x = 2 in
f 3 ;;
```

the lexically scoped `eval_l` evaluator would return 4, similar to `eval_s` which is based on substitution semantics. In contrast, the dynamically scoped evaluator `eval_d` would return 5.

To implement `eval_l`, I started by first copying `eval_d` and changing the evaluation of `Fun` to return type `Env.Closure` pairing the function itself and the environment at time of definition, and changing the evaluation of `App` to evaluate the body of the function in the lexical environment from the corresponding closure. 

However, since `eval_s` and `eval_d` were implemented earlier to return type `expr`, I had to wrap `eval_l` around `eval_l'` which returned type `Env.Val`, now that closures were involved. The value returned from `eval_l'` is then pattern matched to extract only the expression part which is then given as output from `eval_l`.

Since `eval_d` and `eval_l` have different type signatures, I have chosen to leave them separate, instead of attempting to merge the two implementations.