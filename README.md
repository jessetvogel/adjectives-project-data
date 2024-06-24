This repository contains the data for the [Adjectives Project](https://github.com/jessetvogel/adjectives-project).

All types, adjectives, examples and theorems are stored in [YAML](https://yaml.org/) files.

## File structure

Every type, adjective, example and theorem has an `id`, which is given by the filename without the `.yaml` file extension. For instance, the example declared in the `Spec-ZZ.yaml` file is given the id `Spec-ZZ`. One can refer to a type, adjective, example or theorem using that object's `id`.

Every object also has a `name`, which is the displayed name. Providing a `name` is optional, it is by default set to `id`.

Every object may optionally also have a `description`, which for instance could contain a precise definition or proof, or a reference to such.

### Types
A file declaring a type should have the following format. Note that some types may depend on parameters. For instance, a `morphism` has a `source` and a `target`, both of which are schemes.

```yaml
# Type file (morphism.yaml)
type: type
name: morphism
parameters:
  source: scheme
  target: scheme
description: A morphism is [...]
```

### Adjectives

A file declaring an adjective should have the following format.

```yaml
# Adjective file (affine.yaml)
type: scheme adjective # of the form: <type> adjective
name: affine
description: A scheme is affine if [...]
```

Additionally, there may be a field `verb`, whose value is an array of two strings, which indicates how the adjective is used in a sentence. By default, its value is `[is, is not]`. The adjective 'closed immersion' has `[is a, is not a]`, and the adjective 'finite fibers' has `[has, does not have]`.

### Examples

A file declaring an example should have the following format. Arguments are provided through the `with` field. Adjectives of the example may be declared with or without proof.

```yaml
# Example file (Spec-QQ-to-Spec-ZZ.yaml)
type: morphism
name: $\Spec \QQ \to \Spec \ZZ$ # NOTE: name may also include LaTeX
with:
  source: Spec-QQ
  target: Spec-ZZ
adjectives:
  affine: true # without proof
  flat: [true, Localizations are flat.] # with proof
description: The natural map from $\Spec \QQ$ to $\Spec \ZZ$.
```

### Theorems

A file declaring a theorem should have the following format. Propositions (as in the `if` and `then` fields) are of the form `<object> <adjective>` or `<object> not <adjective>`. To refer to an argument of the subject, we use a `.`. For instance, `f.source` refers to the source of `f`. The `if` field must contain a proposition, or an array thereof. The `then` field may only contain a single proposition.

```yaml
# Theorem file (af-of-src-af-trg-af.yaml)
type: theorem
name: morphisms with affine source and target are affine
given: morphism f # of the form: <type> <symbol>
if: [f.source affine, f.target affine]
then: f affine
```

Additionally, there may be a field `converse`, which if set to `true`, indicates that the converse of the theorem also holds (that is, one may swap the conditions and conclusions).

## Conventions 

- The `id` of an object is written in [kebab-case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case).
- The `name` of a theorem is as much a slogan as possible: short and descriptive.
