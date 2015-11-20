# Function

## Define a simple function

Syntax:

```clj
(defn fn-name
  [arg1 arg2] ;; ... means multiple or empty args, if it's emtpy then []
  ret)        ;; means the return value
```

Example:

```clj
(defn add
  [a b]
  (+ a b))

(add 1 2)
```

## Define a simple pattern matching function

Syntax:

```clj
(defn fn-name
  ([matchine1] ret1)
  ([matching2 matching] ret2)
  ...)
```

Example:

```clj
(defn add
  ([a] a)
  ([a b] (+ a b))
  ([a b c] (+ (+ a b) c))
```

## Define a dynamic variable number function

Syntax:

```clj
(defn fn-name
 [a & rest]
 (doo-sth a rest)) ;; can do nothing, just a return value
```

Example:

```clj
(defn first
  [fst & rest]
  fst)
```

## Define a key based function

Syntax:

```clj
(defn foo
  [& {:keys [key1 key2 ...]                   ;; key will be listed
      :or {key1 "default value for key 1"
           key2 "default value for key 2"}}]
  (str key1 "-" key2)) 
```

Example:

```clj
(defn foo
  [& {:keys [bar baz]
      :or {bar "default1"
           baz "default2"}}]
  (str bar "-" baz))

(foo) ;; => "default1-default2"
(foo :bar 1) ;; => "1-default2"
(foo :bar 1 :baz 2) ;; => "1-2"
```

## Define a high-level function - Polymorphism

see - https://clojuredocs.org/clojure.core/defmethod
