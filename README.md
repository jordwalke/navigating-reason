# Navigating Reason

This document is a guide for the curious newcomer to [Reason](https://reasonml.github.io/), or for people looking to branch out of the most common workflows for Reason. Or maybe you're just curious about what tools are being worked on, and how they all relate. How could they converge?
If you're brand new to Reason and just want to get started as quickly as possible with compiling to JS, just go through the main [docs](https://reasonml.github.io/). This document isn't meant to be the best newcomer entrypoint. It's meant for those who want to branch out, or those who want to help contribute to the eventual consolidation of a lot of these tools.

You've at least dabbled in Reason, and you've noticed that Reason can do much more than just compile to JavaScript. How do you do that, what tools are required, and what do you need to learn?

First, let's clarify some terms.

- **Package Manager**:(Example: npm) A tool that solves dependency versions and tells each package to prepare itself.
- **Build System**: (Example: Webpack) A tool that generates artifacts for one package at a time, but that might be able to look at your dependencies' artifacts while doing so.
- **Transpiler**: (Example: Babel) A tool that turns one file into another processed file.

