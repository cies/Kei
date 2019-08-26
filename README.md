# Kei Language

Kei is a dependently typed language with a small and expressive core based on λΠ-calculus modulo rewriting.


## The Core

The core of Key is based on a type theory called *Lambda-Pi-Calculus Modulo Calculus*. Despite the core being very experimental, Kei can prove
some properties through the encoding of a typed rule.

Rules of static symbols are defined as in [Dedukti](https://github.com/Deducteam/Dedukti), for example, a sized list vector can be defined like:

```
Rule Vector : (forall (x : nat) -> Type).
Rule Nil : (Vector Z).
Rule Cons : (forall (x : nat) (y : A) (H : (Vector x)) -> (Vector (S x))).
```

Rewrite rules are expressed like:

```
tail = (\(forall (n' : nat) (vec : (Vector n')) -> Maybe) | x vec => [
  vec of Maybe
    |{x' y H}(Cons x' y H) => (Surely x' H)
    |{}Nil => Nothing
]).
```


One of the most interesting properties of Kei is you can combine static symbols with rewrite rules to create another logic system, like COC. In λΠ-calculus modulo the conversion of terms is available between β-reduction and Γ-Reduction, this means that a type can be changed through a type relation of a rewrite rule. Of course, if there is a well-typed substitution rule σ(x). 


## Basic

For example let's define syntactical equality:

```
Rule type : Type.
Rule ≡ : (forall (n : type) (n' : type) -> Type).
Rule refl : (forall (n : type) -> (≡ n n)).
```

Extending the symbols with a static type with a scheme for proving:

```
Rule eq_rect : (forall (n : type)
                       (n' : type)
                       (x : (≡ n n'))
                       (a : type)
                       (f : (forall (a : type) (a' : type) -> Type))
                       (H : (f a n))
                       ->
                       (f a n')).   

f_sym = (\(forall (x : type) (y : type) -> Type) | x y => (≡ y x)).
symmetry = (\(forall (x : type) (y : type) (H' : (≡ x y)) -> (≡ y x)) 
   | x y H' => (eq_rect x y H' x f_sym (refl x))).
```
Is specifying a symbol scheme required for proofs? While different approaches and logic systems are allowed, the small core of Kei is expressive enough for represent both trivial and more complex proofs (like induction proofs with several static symbols and rewrite rules through rule composition).


## Installation

The GHC is used to build Key:

```
cd ~
git clone https://github.com/caotic123/Kei
cd Kei/src
ghc --make Checker.hs -o Kei

# put the resulting executable in your $PATH 
export PATH="$PATH:~/Kei/src"
```

## Run some examples

Go to the `examples/` folder and run:

```
Kei foo
```

If everything is okay you should see:

```
Bar : Just Foo.
```

The `Just` means that Kei *just* can infer the construction evaluated. So, when you check the terms Kei automatically evaluates the EVAL expression and return the value.


## Rules

![Rules](https://i.imgur.com/zdBnyGI.jpg)  
<sub><sup> (Source : Typechecking in the lambda-Pi-Calculus Modulo : Theory and Practice) </sub> </sub>


## What's not implemented

- Totally Checker
- Confluent Pattern Matching (avoid non Left-Linear Rules), this topic is a bit complicate Dedukti do a optimization of -  patterns matching to solve this.
- Impossible clause
- Patterns matching clauses checking
- Confluent Checker
- A backend :)
- *Fast* type checking


## Inspiration

Kei is influenced by:

* Typechecking in the lambda-Pi-Calculus Modulo: Theory and Practice (Ronan Saillard).  

* The λΠ-calculus Modulo as a Universal Proof Language (Mathieu Boespflug1, Quentin Carbonneaux2 and Olivier Hermant3).  

* Dedukti: a Logical Framework based on the λΠ-Calculus Modulo Theory (Ali Assaf1, et al).  

Other aspects of Kei's language design, like syntax, were defined with the help of [Lucas](https://github.com/luksamuk) and [Davidson](https://github.com/davidsonbrsilva).


```
