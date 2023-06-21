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
- Ghent, Belgium 🇧🇪
- <logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)
- <logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)
- <mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)

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

# &lt;template&gt; tag components in a nutshell

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

```gjs
import Component from '@glimmer/component';
import { tracked } from '@glimmer/tracking';
import { action } from '@ember/object';
import { on } from '@ember/modifier';

export default class CopyToClipboard extends Component {
  @tracked isCopied = false;

  @action
  async copyToClipboard() {
    await navigator.clipboard.writeText(this.args.text);
    this.isCopied = true;
  }

  <template>
    <button {{on 'click' this.copyToClipboard}}>
      {{if this.isCopied 'Copied!' 'Click to copy text'}}
    </button>
  </template>
}
```