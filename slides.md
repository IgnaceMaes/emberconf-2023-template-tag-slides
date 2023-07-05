---
theme: apple-basic
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Getting Started with &lt;template&gt; Tag Components
layout: intro
fonts:
  sans: "Inter"
  mono: "JetBrains Mono"
---

# Getting Started with &lt;template&gt; Tag Components

<span class="opacity-60 font-400 block mt-8">EmberConf 2023 - Portland, OR</span>

<div class="absolute bottom-10">
  <span class="font-700">
    By Ignace Maes
  </span>
</div>

<!--
Presenter notes
-->

---
layout: speaker
image: 'images/me.jpeg'
---

# I'm Ignace Maes <twemoji-waving-hand /> 

Senior Full-Stack Engineer at OTA Insight <br/>
From Ghent, Belgium <!-- <twemoji-flag-belgium />  -->

<logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)<br/>
<logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)<br/>
<mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)<br/>

<!-- <img src="/images/otainsight.svg" class="absolute bottom-16 left-16 w-40 rounded shadow" /> -->

---
layout: image

image: 'images/morpheus.jpeg'
---

<div class="my-auto text-center w-200 mx-auto">
  <h1>
    What if I told you<br/><br/><br/><br/><br/><br/><br/>
    there's a new component format in Ember you can use today?
  </h1>
</div>

<style>
.slidev-layout {
  position: absolute;
  height: 100%;
  width: 100%;
}

.slidev-layout h1 {
  font-size: 3rem;
  line-height: 1;
  font-weight: 700;
  text-shadow: 2px 2px #000000;
}
</style>

---
layout: section
---

# Ember's next Edition

---
layout: image-right
image: 'https://images.unsplash.com/photo-1606536387965-7a709e4559fe?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2940&q=80'
---

### Ember's next Edition
# Pillars of Polaris

- Embroider build system
- First-class TypeScript support
- New Router
- Reactivity
- &lt;template&gt; tag components

---
layout: image-left
image: 'images/recommended.png'
---

### Ember's next Edition

# State of template tag components

We expect to recommend `<template>` as the default authoring format during Ember 5.x.

<!-- 
You don't need to wait until an edition is complete to start using it.
 -->

---
layout: statement
---

# Should you start using &lt;template&gt; tag components today?

---
layout: section
---

# Why a new component format?

<!-- 
What got it to the point of needing a new component format.
 -->

---
layout: image-right
image: 'https://images.unsplash.com/photo-1613951085587-cfe5d0a6cffc?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1803&q=80'
---
### Why a new component format?
# Strict mode templates

- The implementation of strict mode templates
  - No implicit globals, this fallback, invocation of argument-less helpers
  - No dynamic resolution
  - No evals (no partials)

**<twemoji-bullseye /> Goal:** Have everything statically resolvable

**<twemoji-exclamation-question-mark /> Problem:** How should imports be done in templates?

::right::

<!-- 
Before explaining &lt;template&gt; tag components we should take a step back and see where it's coming from.

- JavaScript has always been the wild west
- Time to grow up

TODO: Why? Benefits?
 -->


---
layout: four-sections
---

Demo of button

::topright::

```hbs{1|0}
<CopyToClipboard @text={{"Hello EmberConf 2023!"}} />
```
<div class="opacity-50 text-xs text-center mt-2">application.hbs</div>
<br />

<v-click>

```hbs
<button {{on 'click' this.copyToClipboard}}>
  {{if this.isCopied 'Copied!' 'Click to copy'}}
  <Icon @name={{'clipboard'}} />
</button>
```
<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.hbs</div>
<br />

```js
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  copyToClipboard = async () => {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }
}
```
<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.js</div>

</v-click>

<!-- 
Problem:
- where does Icon component come from: own app, addon, ...?
- where does the modifier come from: own, addon?
 -->

---
layout: four-sections
---

<kbd>#1</kbd> Imports only via frontmatter

```hbs{1-4|0}
---
import Icon from 'example-app/components/icon';
import { on } from '@ember/modifier';
---
<button {{on 'click' this.copyToClipboard}}>
  {{if this.isCopied 'Copied!' 'Click to copy'}}
  <Icon @name={{'clipboard'}} />
</button>
```
<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.hbs</div>
<br />

```js{0}
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  copyToClipboard = async () => {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }
}
```
<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.js</div>

::topright::

<v-click>

<kbd>#2</kbd> Single File Component (Vue/Svelte type)

```vue{1,4-5,15}
<script>
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
import Icon from 'example-app/components/icon';
import { on } from '@ember/modifier';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  copyToClipboard = async () => {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }
}
</script>

<button {{on 'click' this.copyToClipboard}}>
  {{if this.isCopied 'Copied!' 'Click to copy'}}
  <Icon @name={{'clipboard'}} />
</button>
```
<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.glimmer</div>

</v-click>

---
layout: four-sections
---

<!-- # Proposals
### Ember's next Edition -->


<kbd>#3</kbd> Template literals

```js{1,3,4,14-19|0}
import Component, { hbs } from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
import Icon from 'example-app/components/icon';
import { on } from '@ember/modifier';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  copyToClipboard = async () => {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }

  static template = hbs`
    <button {{on 'click' this.copyToClipboard}}>
      {{if this.isCopied 'Copied!' 'Click to copy'}}
      <Icon @name={{'clipboard'}} />
    </button>
  `
}
```

<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.js</div>

::topright::

<v-click>

<kbd>#4</kbd> Template tag component

```gjs{3,4,14-19}
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
import Icon from 'example-app/components/icon';
import { on } from '@ember/modifier';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  copyToClipboard = async () => {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }

  <template>
    <button {{on 'click' this.copyToClipboard}}>
      {{if this.isCopied 'Copied!' 'Click to copy'}}
      <Icon @name={{'clipboard'}} />
    </button>
  </template>
}
```

<div class="opacity-50 text-xs text-center mt-2">copy-to-clipboard.gjs</div>

</v-click>

<!-- 
Template literals - tagged templates
- "The tag function can then perform whatever operations on these arguments you wish, and return the manipulated string"
- fairly unknown JS feature
- create custom identifier in front of template literal to run through a function
 -->

---

### Why a new component format?
# Only one can be the winner

- All solutions solve the **template import problem**
- Different trade-offs to be made in
  - Semantics
  - Learning
  - Tooling
- `<template>` tag components came out as best overall
- Since RFC #779 the official path forward

<br/>
<br/>
<br/>
<br/>

See [Ember Template Imports blog series](https://v5.chriskrycho.com/journal/ember-template-imports/) by Chris Krycho

<!-- 
Choosing is loosing

https://rfcs.emberjs.com/id/0779-first-class-component-templates/
 -->

---
layout: section
---

# Using template tag components today

---
---

### Using template tag components today
# Starting with template tags

<br />

```bash
$ pnpm add --save-dev ember-template-imports
```

Allows defining `.gjs` and `.gts` component files

<br/>

> **Compatibility** <twemoji-information class="ml-1"/> 
> 
> - Ember.js v3.27 or above
> - Ember CLI v2.13 or above
> - ember-cli-htmlbars 6.0 or above
> - Node.js v12 or above

<!-- 
Implemented as addon
- provides current built tooling
- Stable!

Will be built in in the future
 -->

---
---

### Using template tag components today
# 3 types of template location

- Template-only components
- Class-based component
- Local assignment

---
layout: section
---

# Getting the most out of template tag components

---

### Getting the most out of template tag components

# IDE setup

- Plugins to install

---
layout: center-with-bottom-notes
---

# Thank you! <twemoji-raising-hands />

::right::

Ignace Maes<br/>
<span class="opacity-60">Senior Full-Stack Engineer at OTA Insight</span>

::left::

<logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)<br/>
<logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)<br/>
<mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)<br/>