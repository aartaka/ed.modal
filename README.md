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

Supported commands:
- q: quit
- a: append text (next prompt) as a new line after the current one
- i: same, but before the current line
- p: print current line
- d: delete current line
- - and +: to move to next or previous line
- j: join the contents (=words) of this line and the next ones
- e: swap the current line contents with the contents of the next one
- =: Get current line number, in unary.
- . iiii: Move to i-th line, in unary.

# Implementation details for the curious ones

- I'm using a `>` cursor for current line marking.
  With it, it's easy to match anything and everything around the cursor, be it for counting (`=`) or modification.
- I introduced a swap/`e`xchange command just because it was extremely intuitive with this notation.
- I used a `Buffer` type tag to avoid premature command reading when processing lines.
  After the processing is done, I rebuild the buffer, put a `Buffer` tag on it, and the editor is ready for new commands.
  Neat trick that made me appreciate type tags that Devine's wiki was suggesting!
- I like arbitrary length lists and use them for e.g. line contents.
  But Modal doesn't make it easy working with lists, preferring tuples and conses instead.
  Which makes sense, but it still puts me off.
