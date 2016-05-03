# Zipper

Why do I want to use zipper?

- I want to `traversing` a tree structure.

But why zipper?

1. zipper is a core library
2. zipper is a solution to the problem of `traversing` a tree
   structure without any implied state.

## clojure.zip

It's not zlib implementation. It's actually the zipper.

### functions

- `zip/vector-zip`
- `zip/down`
- `zip/right`

### Example

Say if we have a `tree structure looks like this`:

```
┌────────────────────────────────────────────────┐
│                  ┌─────┐                       │
│                  │root │                       │
│                  └─────┘                       │
│                     │                          │
│   ┌─────────┬───────┼────────┬───────┐         │
│   ▼         ▼       ▼        ▼       ▼         │
│┌────┐    ┌────┐  ┌────┐   ┌────┐  ┌────┐       │
││ 1  │    │ch1 │  │ 2  │   │ 3  │  │ch2 │       │
│└────┘    └────┘  └────┘   └────┘  └────┘       │
│       ┌─────┴─────┐          ┌───────┼──────┐  │
│       ▼           ▼          ▼       ▼      ▼  │
│    ┌────┐      ┌────┐     ┌────┐  ┌────┐ ┌────┐│
│    │ :a │      │ :b │     │ 40 │  │ 50 │ │ 60 ││
│    └────┘      └────┘     └────┘  └────┘ └────┘│
└────────────────────────────────────────────────┘
```

Which represent in clojure vector is:

```
[1 [:a :b] 2 3 [40 50 60]]
```

So, if we do 

```clj
(zip/vector-zip [1 [:a :b] 2 3 [40 50 60]])
```

It will have result:

```
[[1 [:a :b] 2 3 [40 50 60]] nil]
```

means that currently we pointing to root:

```
┌────────────────────────────────────────────────┐
│                  ┌─────┐                       │
│                  │root*│                       │
│                  └─────┘                       │
│                     │                          │
│   ┌─────────┬───────┼────────┬───────┐         │
│   ▼         ▼       ▼        ▼       ▼         │
│┌────┐    ┌────┐  ┌────┐   ┌────┐  ┌────┐       │
││ 1  │    │ch1 │  │ 2  │   │ 3  │  │ch2 │       │
│└────┘    └────┘  └────┘   └────┘  └────┘       │
│       ┌─────┴─────┐          ┌───────┼──────┐  │
│       ▼           ▼          ▼       ▼      ▼  │
│    ┌────┐      ┌────┐     ┌────┐  ┌────┐ ┌────┐│
│    │ :a │      │ :b │     │ 40 │  │ 50 │ │ 60 ││
│    └────┘      └────┘     └────┘  └────┘ └────┘│
└────────────────────────────────────────────────┘
```

If we move the cursor down using `zip/down`

```clj
user> (->  [1 [:a :b] 2 3 [40 50 60]]
     zip/vector-zip
     zip/down)
```

It will have result:

```clj
[1 {:l [], :pnodes [[1 [:a :b] 2 3 [40 50 60]]], :ppath nil, :r ([:a :b] 2 3 [40 50 60])}]
```

Which means that we are in current position:

```
┌────────────────────────────────────────────────┐
│                  ┌─────┐                       │
│                  │root │                       │
│                  └─────┘                       │
│                     │                          │
│   ┌─────────┬───────┼────────┬───────┐         │
│   ▼         ▼       ▼        ▼       ▼         │
│┌────┐    ┌────┐  ┌────┐   ┌────┐  ┌────┐       │
││ 1* │    │ch1 │  │ 2  │   │ 3  │  │ch2 │       │
│└────┘    └────┘  └────┘   └────┘  └────┘       │
│       ┌─────┴─────┐          ┌───────┼──────┐  │
│       ▼           ▼          ▼       ▼      ▼  │
│    ┌────┐      ┌────┐     ┌────┐  ┌────┐ ┌────┐│
│    │ :a │      │ :b │     │ 40 │  │ 50 │ │ 60 ││
│    └────┘      └────┘     └────┘  └────┘ └────┘│
└────────────────────────────────────────────────┘
```

Then we move right using `zip/right`:

```clj
(-> [1 [:a :b] 2 3 [40 50 60]]
    zip/vector-zip
    zip/down
    zip/right)
```

Then it will have result:

```
[[:a :b] {:l [1], :pnodes [[1 [:a :b] 2 3 [40 50 60]]], :ppath nil, :r (2 3 [40 50 60])}]
```

Which means that the cursor move to ch1 node:

```
┌────────────────────────────────────────────────┐
│                  ┌─────┐                       │
│                  │root │                       │
│                  └─────┘                       │
│                     │                          │
│   ┌─────────┬───────┼────────┬───────┐         │
│   ▼         ▼       ▼        ▼       ▼         │
│┌────┐    ┌────┐  ┌────┐   ┌────┐  ┌────┐       │
││ 1  │    │ch1*│  │ 2  │   │ 3  │  │ch2 │       │
│└────┘    └────┘  └────┘   └────┘  └────┘       │
│       ┌─────┴─────┐          ┌───────┼──────┐  │
│       ▼           ▼          ▼       ▼      ▼  │
│    ┌────┐      ┌────┐     ┌────┐  ┌────┐ ┌────┐│
│    │ :a │      │ :b │     │ 40 │  │ 50 │ │ 60 ││
│    └────┘      └────┘     └────┘  └────┘ └────┘│
└────────────────────────────────────────────────┘
```

We can keep going down, using `zip/down`. But I should better stop
here since it's already really clear that what these 3 function do
to the simulated vec-tree.

Instead of doing things manually using `down|right|left` whever, there's a
helper function called `next` which allows us to get the next `cursor` within
the tree using depth first travxxx. And we can use function `zip/node` to get the
element of it.

Let's say if we want to get the `:a` node. We can do:

```clj
(-> [1 [:a :b] 2 3 [40 50 60]]
    zip/vector-zip
    zip/next
    zip/next
    zip/next
    zip/node)
```

Which returns: `:a`

### Update the leaf

```clj
(-> [1 [:a :b] 2 3 [40 50 60]]
    zip/vector-zip
    zip/next
    zip/next
    zip/next
    zip/edit (fn [k v] [k :c])
    zip/root)
```

This will update the node in `:a` to `:c`, then goes
back to the root of the tree and return it.

If we want to check if we zip to the end. We can use
`zip/end?` to check.

So, keep working on my html parsing script now. :D
