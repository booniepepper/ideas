# Ideas

## Improved UTF-8 implementation

While reading through https://nietaki.com/2023/04/21/elixir-string-operations-seem-slow-and-why-its-a-good-thing/

It occurs to me that there are better memory layout and access strategies for UTF-8 unicode.

Idea 1:

String memory layout as chunks. Each chunk contains _X_ runes, and therefore you can
index into the _(N % X)_ chunk when looking to access/split/etc on a given index.

A drawback is that while this is optimal for O(1) _access_ it means actions like removing
or replacing substrings will require a lot of recalculation O(N).

Idea 2:

Keep a list/map of length-at indices. Searches through a string will cache the length at
specific indices, and subsequent searches can translate a rune length for a human to a
string length at O(1). Mutation operations can choose to either invalidate or recalculate
indices.

Idea 3:

Encode strings as runelength + rune. Still requires forward traversal and O(N)
for almost all string operations, but more can be skipped when traversing.

Idea 4:

Advanced data structures like suffix trees (thanks @andrewCodeDev for introducing me to these)
but why not make that a default String type? It will not be ideal for small text
but will be ideal for large text and complex text operations
