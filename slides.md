---
marp: true
lang: en
title: Biome toolchain of the web
description: Biome toolchain of the web
author: Victorien Elvinger
transition: fade
paginate: true
theme: gaia
style: |
  .columnar{ display: flex; }
  .columnar * { flex: 1;
  }
  section {
    padding: 15px 30px;
  }
  aside {
    position: absolute;
    top: 110px;
    right: 45px;
    height: 620px;
  }
---

<!--
_class: lead
_paginate: skip
-->

![h:200px](./assets/biome-logo.svg)

**Victorien Elvinger**
_@Conaclos_
Biome lead maintainer

---
<!--
_class: lead invert
_paginate: hide
-->

# What is **Biome**?

---

## A code **linter**

- JavaScript, **TypeScript, JSX**, TSX
  - no extra dependencies

- **Helpful diagnostics**

- **200 lint rules**
  - some unique to Biome
  - ESLint, ESLint plugins
  - 🚧 Tailwind class sorting

![bg right:50% w:100%](./assets/linter-diagnostic.drawio.svg)

---

![bg right:51% w:100%](./assets/biome-format-invalid.webp)

## A code **formatter**

- JavaScript, TypeScript, JSX, TSX
- JSON, JSONC
- 🚧 CSS
- **format invalid code**

---

<!--
_class: lead invert
_paginate: hide
-->

![bg w:68%](./assets/tweet-vjeux-10kbounty.png)

---

<!--
_class: lead invert
_paginate: hide
-->

![bg](./assets/prettier-challenge.png)

---

<!--
_class: lead invert
_paginate: hide
-->

![bg w:68%](./assets/tweet2.png)

---
<!--
_class: lead invert
_paginate: hide
-->

# Is Biome **fast**?

---


<!--
_class: lead invert
_paginate: hide
-->

![bg w:68%](./assets/tweet1.png)


---

## A **community**

- ⬇️ 170k weekly downloads
- ⭐ 8.4k GitHub Stars
- 🐦 4.6k followers
- 💬 1.2k Discord members

![bg h:80%](./assets/biome-users.drawio.svg)


---
<!--
_class: lead invert
_paginate: hide
-->

# How Biome works?


---

![bg right:55% w:85%](./assets/architecture.drawio.svg)

## Architecture

- **leader-follower**
- the leader thread
  - spawn a thread per file
  - collect results
- a follower thread
  - parse the given file
  - handle (format, lint)

---

## Regular parser

![bg right:45% h:95%](./assets/parse-ast.drawio.svg)

1. parse to an Abstract Syntax Tree
2. handle (format, lint)

---

![bg right:45% h:95%](./assets/parser-syntax-error.drawio.svg)

## Regular parser

- doesn't handle invalid code
  - **emit syntax error**

---

![bg right:45% h:95%](./assets/parse-cst.drawio.svg)

## **Biome** parser

- **accept invalid code**
  - bogus tree nodes
  - holes in the tree

- **lossless** parsing using CST
  - preserve whitespace


---

![bg right:45% h:95%](./assets/parse-cst-fix.drawio.svg)

## **Biome** parser

- **accept invalid code**
  - bogus tree nodes
  - holes in the tree

- **lossless** parsing using CST
  - preserve whitespace

---

![bg right w:80%](./assets/name-resolver/js-unused-var.drawio.svg)

## Lint rules

- many rules query the tree
  - noVar
  - noDoubleEquals
  - noAccumulatingSpread

- others need more complex data
  - **noUnusedVariables**
  - noUnusedImports
  - useImportType
  - useExportType


---

![bg right h:85%](./assets/parse-semantic-lint.drawio.svg)

## Semantic model

- find references of a declaration
  - write refrences
  - read references

---

![bg right w:80%](./assets/name-resolver/js-unused-var-names.drawio.svg)

## Name resolver v1

- bind declarations to references
  - unique id for each declaration
  - a reference refers to a single declaration
- take scopes into account
  - variable shadowing


---

![bg right w:100%](./assets/name-resolver/name-resolver-v1-ts.drawio.svg)

## TypeScript 🤯


- **type & variable** with **same name**


---

![bg right w:100%](./assets/name-resolver/ts-muli-decl-ref.drawio.svg)


## TypeScript 🤯

- type & variable with same name
- **a reference** can refer to a **type** and a **variable**

---

![bg right w:55%](./assets/name-resolver/ts-multi-decl-ref-2.drawio.svg)

## TypeScript 🤯🤯

- type & variable with same name
- ~~a reference can refer to a type and a variable~~
- **a reference** can refer to **multiple declarations**

---

![bg right w:70%](./assets/name-resolver/ts-type-val-duality.drawio.svg)

## TypeScript 🤯

- type & variable with same name
- ~~a reference can refer to a type and a variable~~
- a reference can refer to multiple declarations
- **partially referenced declarations**
  - type / variable duality

---

![bg right w:70%](./assets/name-resolver/reality-model.drawio.svg)

## Simplification

- a reference refers to a **single declaration**
  - handle differently edge cases (_export_, _infer_)
- type and value with same names
- type / variable duality

---

![bg right w:95%](./assets/name-resolver/name-resolver-v2.drawio.svg)

## Name resolver v2

- A declaration is either
  - a type
  - a variable
  - both
- A reference refers either
  - a type
  - a varaible


---

![bg right w:95%](./assets/name-resolver/name-resolver-v2-api.drawio.svg)

## Name resolver v2

- A declaration is either
  - a type
  - a variable
  - both
- A reference refers either
  - a type
  - a varaible
- type/value duality not exposed


---

## Conclusion

- Biome is both a **formatter** and a **linter**
  - and more: JavaScript **import sorting**
- Biome is **fast**
- Biome is **editor-ready**
  - error-resilient parsers
  - Concrete Syntax Tree
- Biome **supports TypeScript**
  - type-aware semantic model

---

## 2024 and beyond

- extend to **more languages**
  - CSS, HTML, Markdown
  - Vue, Angular, Svelte, Astro
- **improve linter capabilities**
  - multi-file analysis
  - simplified type system
- 🧱 **plugins**

---

## Want to help?

- 🚀 try Biome
  - 🐛 report issues
  - 💬 feedbacks
- contribute to Biome
  - [GitHub good first issues](https://github.com/biomejs/biome/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
  - 🎥 [How to create a lint rule in Biome](https://www.youtube.com/watch?v=zfzMO3nW_Wo)
  (youtube.com/@Biomejs)
- 💸 sponsor us!
  - [Biome Open Collective](https://opencollective.com/biome)


---

<!--
_class: lead
_paginate: hide
-->

![h:150px](./assets/biome-icon-light-transparent.svg)

# biomejs.dev

<small>

_format code_
npx **@biomejs/biome format --write** src

_lint code, apply safe fixes_
npx **@biomejs/biome lint --apply** src

_all at once_
npx **@biomejs/biome check --apply** src

</small>

---
<!--
_class: lead invert
_paginate: hide
-->

# Backup slides

---

## A toolchain

- 🧰 toolchain for **web dev**
  - code formatter
  - code linter
- written in **Rust** 🦀
- supports main web language
  - **JavaScript, TypeScript, JSX, TSX**
  - JSON, JSONC
  - CSS ⌛️
- **community successor** of Rome Tools

---

## A **fast** formatter

- scales with available threads

</br>

![w:1200px](./assets/biome-vs-prettier.svg)

---

![bg right:50% h:60%](./assets/governance.drawio.svg)

## A **governance**

- leads (2) 🔑 owners
  - 🛡️ access to sensible data
  - ⚔️ act as tiebreakers

- core contributors (5)
  - 👍 project directions

- maintainers (5)
  - 👍 project decisions
  - ⬆️ write access to the repo
