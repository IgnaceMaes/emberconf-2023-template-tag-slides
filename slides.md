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

<span class="text-white/60 font-400 block mt-8">EmberConf 2023 - Portland, OR</span>

<div class="absolute bottom-10">
  <span class="font-700">
    By Ignace Maes
  </span>
</div>

<!--
Presenter notes
-->

---
layout: image-right
image: 'images/me.jpeg'
---

# I'm Ignace Maes 👋

- Senior Full-Stack Engineer at OTA Insight
- From Ghent, Belgium 🇧🇪
- <logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)
- <logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)
- <mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)

<img src="/images/otainsight.svg" class="absolute bottom-16 left-16 w-60 rounded shadow" />

---
layout: statement
---

# Should you start using &lt;template&gt; tag components today?

---
layout: image-right
image: 'https://images.unsplash.com/photo-1606536387965-7a709e4559fe?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2940&q=80'
---

# Pillars of Polaris
### Ember's next Edition

- Embroider build system
- &lt;template&gt; tags components
- First-class TypeScript support
- New Router
- Reactivity

---
layout: section
---

# &lt;template&gt; tag components, you say?

---
layout: two-cols
---

# Template tag components
### In a nutshell

- The implementation of strict mode templates
  - No implicit globals, this fallback, invocation of argument-less helpers
  - No dynamic resolution
  - No evals (no partials)
- New `.gjs` and `.gts` file formats
- Allows embedding `<template>` tags in JavaScript/TypeScript

::right::

```hbs
<CopyToClipboard @text={{"Copy me"}} />
```

<br />

```gjs
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
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
    </button>
  </template>
}
```

<!-- 
Before explaining &lt;template&gt; tag components we should take a step back and see where it's coming from.

- JavaScript has always been the wild west
- Time to grow up
 -->

---
layout: four-sections
---

<!-- # Proposals
### Ember's next Edition -->


Template tag component

```gjs
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
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
    </button>
  </template>
}
```

<div class="text-white/50 text-xs text-center mt-2">example.gjs</div>

::topright::

Template literals

```js
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
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
    </button>
  `
}
```

<div class="text-white/50 text-xs text-center mt-2">example.js</div>

::bottomleft::

Single File Component (Vue/Svelte type)

```vue
<script>
  import Component from '@glimmer/component';
  import { tracked } from '@glimmer/tracking';

  export default class SetUsername extends Component {
    @tracked name;

    get nameValue() {
      return this.name ?? this.args.name;
    }
  }
</script>

<form {{on "submit" this.saveName}}>
  <button type='submit' disabled={{eq this.value.length 0}}>
    Generate
  </button>
</form>
```

::bottomright::

Imports only via frontmatter

```hbs
---
import { on } from '@ember/modifier';
---

<form {{on "submit" this.saveName}}>
  <label for='name'>Set username:</label>
  <input
    id='name'
    value={{this.value}}
    {{on "input" this.updateName}}
  />
  <button type='submit' disabled={{eq this.value.length 0}}>
    Generate
  </button>
</form>
```

---
layout: center-with-bottom-notes
---

# Thank you!

::right::

Ignace Maes<br/>
<span class="opacity-60">Senior Full-Stack Engineer at OTA Insight</span>

::left::

<logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)<br/>
<logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)<br/>
<mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)<br/>