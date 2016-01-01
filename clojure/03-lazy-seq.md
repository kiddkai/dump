# Sequence can be lazy

What is lazy and what isn't?

### Lazy Sequence

Calculate the result when needed, for example:

```clj
(take 5 (range))
```

will not have infinity loop :D

### Regular Sequence

Calculate all the results

## Ref

http://theatticlight.net/posts/Lazy-Sequences-in-Clojure/
http://clojure.org/lazy
