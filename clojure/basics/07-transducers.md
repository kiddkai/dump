# transducers

> trancuders is not just simple `map` or `functor` to just do a simple transform

## So what is transducers

From official docs:

    Transducers are composable algorithmic transformations. They are independent from the context of their input and output sources and specify only the essence of the transformation in terms of an individual element. Because transducers are decoupled from input or output sources, they can be used in many different processes - collections, streams, channels, observables, etc. Transducers compose directly, without awareness of input or creation of intermediate aggregates.

## Transformations

It's about defining a `transducer`. In clojure, `map`, `filter` ... they all
have a way to define a transducer by only pass in one less arguments as ordinary
call.

```js
(filter odd?) ;; returns a transducer that filters odd
(map inc)     ;; returns a mapping transducer for incrementing
(take 5)      ;; returns a transducer that will take the first 5 values
```

So, defining your own transducers using current existing transducers should
be very easy.

```js
(def xf (comp (filter odd?)
              (map inc)
              (take 5)))
```

So the comp looks like the `->>` macro. But internally when we tranduce a list
it will not create extra results in order to pass to the next transducer.

Built-in functions supports creating transducers:

- map
- cat
- mapcat
- filter
- remove
- take
- take-while
- take-nth
- drop
- drop-while
- replace
- partition-by
- partition-all
- keep
- keep-indexed
- map-indexed
- distinct
- interpose
- dedupe
- random-sample

## fns

transduce is not lazy.

```clj
(transduce a-transduce-fn agg-fn [init] collection)
```

```clj
(def iter (eduction xf (range 5)))
(reduce + 0 iter)
```

```clj
(into [] xf (range 1000))
```

```clj
(sequence xf (range 1000))
```

## Create my own transducers

```clj
(fn [xf]
  (fn ([] ...)               ;; init
      ([result] ...)         ;; completion
      ([result input] ...))) ;; step
```

    Init (arity 0) - should call the init arity on the nested transform xf, which will eventually call out to the transducing process.
    Step (arity 2) - this is a standard reduction function but it is expected to call the xf step arity 0 or more times as appropriate in the transducer. For example, filter will choose (based on the predicate) whether to call xf or not. map will always call it exactly once. cat may call it many times depending on the inputs.
    Completion (arity 1) - some processes will not end, but for those that do (like transduce), the completion arity is used to produce a final value and/or flush state. This arity must call the xf completion arity exactly once.

So, in order to create a usable transducer. For example, a `map` xform:

from http://ignaciothayer.com/post/Transducers-Are-Fundamental/

```clj
(defn map
  ([f] ;; When called with a single argument, 
     (fn [f1] ;; map returns a transducer that accepts a reducing function.
       (fn  ;; Like our crude transducer, this transducer returns 
            ;; a reducing function, 
         ([] (f1))
         ([result] (f1 result))
         ([result input]  ;; and this arity does the same as what ours did.
            (f1 result (f input)))
         ([result input & inputs]
            (f1 result (apply f input inputs))))))
```

When if we want to terminate the transducer, we need to returns a
`reduced` value. Which tells the transducer is time to end the
transformation.

For example, the `take` function:

```clj
(defn take
  "Returns a lazy sequence of the first n items in coll, or all items if
  there are fewer than n.  Returns a stateful transducer when
  no collection is provided."
  {:added "1.0"
   :static true}
  ([n]
     (fn [rf]
       (let [nv (volatile! n)]
         (fn
           ([] (rf))
           ([result] (rf result))
           ([result input]
              (let [n @nv
                    nn (vswap! nv dec)
                    result (if (pos? n)
                             (rf result input)
                             result)]
                (if (not (pos? nn))
                  (ensure-reduced result) ;; returns a reduced value
                  result)))))))           ;; returns normal value
  ([n coll]
     (lazy-seq
      (when (pos? n)
        (when-let [s (seq coll)]
          (cons (first s) (take (dec n) (rest s))))))))
```

