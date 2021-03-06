# Compiler Directives

There are a number of directives (instructions to the compiler, beginning with
a `%` symbol) which are not part of the Idris 2 language, but nevertheless
provide useful information to the compiler. Mostly these will be useful for
library (especially Prelude) authors. They are ordered here approximately
by "likelihood of being useful to most programmers".

## Language directives

### %name

Syntax: `%name <name> <name_list>`

For interactive editing purposes, use `<name_list>` as the preferred names
for variables with type `<name>`.

### %hide

Syntax: `%hide <name>`

Throughout the rest of the file, consider the `<name>` to be private to its
own namespace.

### %auto_lazy

Syntax: `%auto_lazy <on | off>`

Turn the automatic insertion of laziness annotations on or off. Default is
`on` as long as `%lazy` names have been defined (see below).

### %runElab

Syntax: `%runElab <expr>`

Run the elaborator reflection expression `<expr>`, which must be of type
`Elab a`. Note that this is only minimally implemented at the moment.

### %lazy

Syntax: `%lazy <lazy_name> <delay_name> <force_name> <infinite_name>`

Use the given names for implicit insertion of laziness annotations. The
`infinite_name` is so that the totality checker can tell the difference between
laziness used for coinduction, and other forms (which are just ignored in
totality checking).

### %pair

Syntax: `%pair <pair_type> <fst_name> <snd_name>`

Use the given names in `auto` implicit search for projecting from pair types.

### %rewrite

Syntax: `%rewrite <rewrite_name>`

Use the given name as the default rewriting function in the `rewrite` syntax.

### %integerLit

Syntax: `%integerLit <fromInteger_name>`

Apply the given function to all integer literals when elaborating.
The default Prelude sets this to `fromInteger`.

### %stringLit

Syntax: `%stringLit <fromString_name>`

Apply the given function to all string literals when elaborating.
The default Prelude does not set this.

### %charLit

Syntax: `%charLit <fromChar_name>`

Apply the given function to all character literals when elaborating.
The default Prelude does not set this.

### %allow_overloads

Syntax: `%allow_overloads <name>`

This is primarily for compatibility with the Idris 1 notion of name overloading
which allows in particular `(>>=)` and `fromInteger` to be overloaded in an
ad-hoc fashion as well as via the `Monad` and `Num` interfaces respectively.

It effect is: If `<name>` is one of the possibilities in an ambiguous
application, and one of the other possibilities is immediately resolvable by
return type, remove `<name>` from the list of possibilities.

If this sounds quite hacky, it's because it is. It's probably better not to
use it other than for the specific cases where we need it for compatibility.
It might be removed, if we can find a better way to resolve ambiguities with
`(>>=)` and `fromInteger` in particular!

## Implementation/debugging directives

### %logging

Syntax: `%logging <level>`

Set the logging level. In general `1` tells you which top level declaration
the elaborator is working on, up to `5` gives you more specific details of
each stage of the elaboration, and up to `10` gives you full details of
type checking including progress on unification problems

