# Re-implementing ed(1) in Modal

[Modal is a beautiful term-rewriting system](https://wiki.xxiivv.com/site/modal.html).
While ed(1) is ugly and stateful.
I wondered if I can marry these and learn Modal by writing ed(1) in it
[as I already did](https://github.com/bf-enterprise-solutions/ed.bf), [more than once](https://github.com/aartaka/ed.bas).
Re-implementing ed(1) is fun!

# Running

Fetch Devine Lu Linvega's [latest Modal](https://git.sr.ht/~rabbits/modal) and run 
``` sh
modal ed.modal
a
hello
p
=> hello
```

# Implementation details for the curious ones

- I'm using a `>` cursor for current line marking.
  With it, it's easy to match anything and everything around the cursor, be it for counting (`=`) or modification.
- I introduced a swap/`e`xchange command just because it was extremely intuitive with this notation.
- Modal (at least the implementation I'm using) is matching toplevel patterns first, which is unintuitive to me as an applicative language programmer.
  I had to replace the editor form (`editor (contents) input`) with service forms like `%insert` and only after full iteration `%merge` them back into `editor`.
  Sometimes that takes even more layers of indirection, like with `%%join`. 
  Not fun.
- I like arbitrary length lists and use them for e.g. line contents.
  But Modal doesn't make it easy working with lists, preferring tuples and conses instead.
  Which makes sense, but it still puts me off.
