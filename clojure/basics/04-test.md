# Testing

> all about testing

## lein test

What is required:

- Module `clojure.test`

```clj
(ns project.namespace-test
  (:require [clojure.test :refer :all]
            [project.namespace :refer :all])) 
```

## Define a test

- use function `deftest` to define a test function
- use function `testing` to give the test a description
- use function `is` to make a actual assertion

```clj
(deftest test-name
  (testing "I should be good"
    (is (= 1 1))))                   ; is takes a boolean to make a assertion
```

## Property based testing

The property-based testing comes from Haskell community called [QuickCheck].
It also have the clojure version here https://github.com/clojure/test.check.

Add `[org.clojure/test.check "0.9.0"]` within the `test` dependencies. Then
it should be useable.

### Not with clojure.test

```clj
(ns project.namespace-test
  (:require [clojure.test.check :as tc]
            [clojure.test.check.generators :as gen]
            [clojure.test.check.properties :as prop]
            [project.namespace :refer :all])) 

(def some-prop
  "define a prop test function"
  (prop/for-all [v (gen/vector gen/int)]
    (= (sort v) (sort (sort v)))))

;; runs the prop test function 100 times
(tc/quick-check 100 some-prop)
```

### With clojure.test

The macro `clojure.test.check.clojure-test/defspec` allows you to succinctly
write properties that run under the `clojure.test` runner.

```clj
(defspec first-element-is-min-after-sorting ;; the name of the test
         100 ;; the number of iterations for test.check to test
         (prop/for-all [v (gen/not-empty (gen/vector gen/int))]
           (= (apply min v)
              (first (sort v)))))
```
