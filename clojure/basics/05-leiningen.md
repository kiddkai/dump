# Leiningen

> automation made easy

https://github.com/technomancy/leiningen/blob/master/doc/TUTORIAL.md

Project.clj will contains informations below:

- Project name
- Project description
- What libraries the project depends on
- What Clojure version to use
- Where to find source files
- What's the main namespace of the app

### Create a new project

    lein new app <name>

Lein will create a project based on a template called `app`, but
where does this `app` template comes from? Still a question for me
yet.

What it will do it's creating a project looks like this:

    ├── CHANGELOG.md 
    ├── LICENSE
    ├── README.md
    ├── doc
    │   └── intro.md
    ├── project.clj - the project metadata file
    ├── resources
    ├── src
    │   └── resume
    │       └── core.clj - the application entry point
    └── test
        └── resume
            └── core_test.clj - the application entry point test

#### project.clj

    (defproject resume "0.1.0-SNAPSHOT"
      :description "FIXME: write description"
      :url "http://example.com/FIXME"
      :license {:name "Eclipse Public License"
                :url "http://www.eclipse.org/legal/epl-v10.html"}
      :dependencies [[org.clojure/clojure "1.7.0"]]
      :main ^:skip-aot resume.core
      :target-path "target/%s"
      :profiles {:uberjar {:aot :all}})

It works pretty similor to `package.json` for npm. But seems
even powerful.

-  "-SNAPSHOT". This means that it is not an official release but a development build.

#### Adding dependency

Leiningen will search `clojars` and `maven` repos for packages, for private registry.
Set the key `:repositories` with a vector of repos.

- `lein search <pkg-name>` for packages
- update the `:dependencies` key in the `project.clj` and run `lein deps`, or restart
  the app using lein, it will automatically fetch the new dependencies and add it to
  the `classpath`(java stuff).

    :dependencies [[org.clojure/clojure "1.7.0"]
                   [other/dep "<version>"]]

What about `devDependencies` in `npm` the package?

Use `profiles`

#### Profiles

    :profiles {:dev {:dependencies [[ring/ring-devel "1.4.0"]]}}

##### Activating Profiles

To activate a different set of profiles for a given task, use the
with-profile higher-order task:


    $ lein with-profile 1.3 test :database

Multiple profiles may be combined with commas:

    $ lein with-profile qa,user test :database

Multiple profiles may be executed in series with colons:

    $ lein with-profile 1.3:1.4 test :database

The above invocations activate the given profiles in place of the
defaults. To activate a profile in addition to the defaults, prepend
it with a +:

    $ lein with-profile +server run

For more details about profiles, run `lein help profiles`.


### Some quick command

Run the project:
    
    lein run

Run tests:

    lein test

Run repl:
    
    lein repl

Install Packages:
   
    lein install

Package the project:
 
    lein uberjar

