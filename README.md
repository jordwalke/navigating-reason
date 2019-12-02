
# Navigating Reason

This document explains the extended set of tools being developed as part of the
Reason project (including community collaborations).  This document is a guide
for the curious, and for people looking to branch out of the most common
workflows for Reason or find out about tools being devloped in various stages.
There are other tools not focused on in this document that overlap in purpose
with some of these tools, but this document will focus on the tools developed
under the Reason umbrella (including community collaborations), all of which
have the ultimate goal of being approachable to the web community and people
coming from JavaScript/React ecosystems.

**If you're brand new to Reason and just want to get started as quickly as
possible with compiling to JS, just go through the main
[docs](https://reasonml.github.io/) instead.**

You've at least dabbled in Reason, and you've noticed that Reason can do much
more than just compile to JavaScript. How do you do that, what tools are
required, and what do you need to learn?

### Terminology:

- **Package Manager**:(Example: npm) A tool that solves dependency versions and
  tells each package to prepare itself.
- **Build System**: (Example: Webpack) A tool that generates artifacts for one
  package at a time, but that might be able to look at your dependencies'
  artifacts while doing so.
- **Compiler**: Lower level tool that powers the language itself, and hopefully
  you never have to actually use this. Usually your build system will invoke
  the compiler. You just enjoy the result.

Additional tools:
- **Transpiler**: (Example: Babel) A tool that turns one file into another processed file.
  - A type of compiler, also known as a source-to-source compiler


### What kind of workflows does Reason currently support?

- **Compiling Reason To Native:**
- **Compiling Reason To JavaScript Via BuckleScript:**

Depending on which of those goals you're pursuing, there is different tooling
required. Ideally the tools for these workflows would be unified into just one.
That may not happen overnight, but we can work towards that ideal state, and this high
level overview will make it clear what needs to happen.  We're not going to
describe that ideal state just yet - for now, let's just describe how things
currently work and which tools are involved.


**The Universe of All Packages is split into two kinds**.

- BuckleScript packages: Packages that can be compiled with BuckleScript, and not be compiled with native toolchains.
- Native packages: Packages that can be compiled with native toolchains, and not be compiled with BuckleScript.


### Tooling For Native:

When developing native packages, you will encounter the following tooling.
We'll also note the analogous concept in the JavaScript/node ecosystem.

|     Tool                                    | Description                                                            | Analogous to JS tool|
|---------------------------------------------|------------------------------------------------------------------------|---------------------|
| [`esy`](https://esy.sh/)                    | Package manager for native Reason.                                     |`npm`                |
| `package.json` or `esy.json`                | Just like with npm `package.json` file. Used to configure dependencies.|`package.json`       |
| [`dune`](https://dune.readthedocs.io/)      | Native Build system for Reason/OCaml.                                  |`webpack`            |
| [`pesy`](https://github.com/esy/pesy)       | Helps create new project, and generates dune config.                   |`create-react-app`   |
| [`merlin`](https://github.com/ocaml/merlin/)| Provides autocomplete support in editors.                              |                     |
| `"esy": {"build": "my command"}`            | The command in your `package.json` that will be run when your package is developed or installed as a dependency. Usually this command just tells `dune` to perform the build. |`"postinstall": "build command"`   |
| "ppx"                                       | Tools that transform your source code at build time. These are per-file transforms.                              |Babel plugins.                     |
|                                             |                                                                        |                     |


How does using those those tools for native development differ than when developing BuckleScript projects?

|                                | BuckleScript Packages                                                                         | Native Packages                                                                                                                      |
|--------------------------------|-----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------
| Package Manager:               | npm                                                                                           | esy                                                                                                                                  |
| Package Config:                | `package.json`                                                                                | `package.json` (or `esy.json`)                                                                                                       |
| Can Use Dependencies:          | JS dependencies or other BuckleScript packages.                                               | Native packages                                                                                                                      |
| Dependencies Hosted:           | On npm.                                                                                       | On opam `"@opam/pkg-name"`, or on npm `"native-pkg-hosted-on-npm"`                                                                   |
| Project Install+build          | `npm install`<br>`bsb`                                                                        | `esy`                                                                                                                                |
| Build System                   | `bsb` (included with bs-platform)                                                             | [Dune](https://dune.readthedocs.io/)                                                                                                 |
| Required Project Dependencies: | `bs-platform`                                                                                 | `ocaml`, `dune`                                                                                                                      |
| Build Config                   | `bsconfig.json`                                                                               | One `dune` config file per directory in project.                                                                                     |
| Compiler:                      |│ Reason Syntax<br>│ OCaml Type System<br>v BuckleScript JS backend<br> `.js` files            |│ Reason Syntax<br>│ OCaml Type System<br>v Native OCaml backend<br> `.exe`                                                           |
| Simplified Workflow:           | `npm install -g bs-platform`<br>`bsb -init`                                                   | `npm install -g esy pesy`<br>`pesy`                                                                                                  |



### Advanced Common Workflows:
- **Creating a plain npm package that contains a ppx plugin:**
  - This is useful because it allows BuckleScript projects to be able to use
    ppx plugins, even though usually ppx plugins are a "native package"
    feature.
  - But to make this work, you have to create prebuilt binaries and publish them to npm.
  - This process involves taking a native package, prebuilding it and then consuming it from
    BuckleScript as a ppx plugin. Recall that most of the time native packages and Bucklescript
    packages are incompatible. Prebuilding them makes it compatible - but it only works in this
    one case of building a ppx plugin.


### Improving Things:

To simplify everything, here are some things you can help out with to help us
consolidate tooling and remove confusion/complexity.

1. Make it so that you can develop pure BuckleScript applications using `esy`, but keep everything else the same.
  - This means just replacing `npm` with `esy`, but still using the rest of the BuckleScript tools.
  - This is beneficial because then you only need one package manager to do
    both native and BuckleScript development.
2. Make it so that there is one editor plugin for each editor that can work
   with `esy` for native and `esy` for BuckleScript projects.

