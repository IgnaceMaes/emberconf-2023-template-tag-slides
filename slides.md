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

<span class="opacity-60 font-400 block mt-8">By Ignace Maes</span>

<div class="absolute bottom-10">
  <span class="font-700">
    EmberConf 2023 - Portland, OR
  </span>
</div>

<!--
Presenter notes
-->

---
layout: speaker
image: '/images/me.jpeg'
---

# I'm Ignace Maes <twemoji-waving-hand /> 

Senior Full-Stack Engineer at OTA Insight <br/>
From Ghent, Belgium <!-- <twemoji-flag-belgium />  -->

<logos-github-icon style="filter: brightness(0) invert(1);"/> [@IgnaceMaes](https://github.com/IgnaceMaes)<br/>
<logos-twitter /> [@Ignace_Maes](https://twitter.com/Ignace_Maes)<br/>
<mdi-web /> [www.ignacemaes.com](https://ignacemaes.com/)<br/>

<!--
img src="/images/otainsight.svg" class="absolute bottom-16 left-16 w-40 rounded shadow"
-->

---
layout: image

image: '/images/morpheus.jpeg'
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
layout: four-sections
---

### Ember's next Edition
# Pillars of Polaris

::topleft::

- Embroider build system
- First-class TypeScript support
- New Router
- Reactivity
- &lt;template&gt; tag components

::topright::

![octane](/images/octane.png)

<!-- 
- Can we get a Polaris Tomster?
-->

---

### Ember's next Edition
# Ember 5.0 release post

<img src="/images/recommended-highlight.png" />

<!-- 
You don't need to wait until an edition is complete to start using it.
 -->

---
layout: statement
---

# Should you start using &lt;template&gt; tag components today?*

<br />
<v-click>

\* We'll get back to this at the end

</v-click>

---
layout: section
---

# Why a new component format?

<!-- 
What got it to the point of needing a new component format.
 -->

---

### Why a new component format?
# Strict mode templates

- **Goal:** make components statically resolvable
- **Benefits**
  - Better for bundling and build tools
  - Simplifies Ember internals and offers better performance
  - Flexible layout for file structure

<br />

**<twemoji-exclamation-question-mark /> Problem:** How should imports be done in templates?

::right::

<!-- 
Strict mode:
  - No implicit globals, this fallback, invocation of argument-less helpers
  - No dynamic resolution
  - No evals (no partials)

Before explaining &lt;template&gt; tag components we should take a step back and see where it's coming from.

- JavaScript has always been the wild west
- Time to grow up

TODO: Why? Benefits?
 -->


---
layout: four-sections
---

::topleft::

# Live example

Text: Hello EmberConf 2023!

<copy-to-clipboard />

::topright::

```hbs{1|0}
<CopyToClipboard @text={{"Hello EmberConf 2023!"}} />
```
<file-name>application.hbs</file-name>
<br />

<v-click>

```hbs
<button {{on 'click' this.copyToClipboard}}>
  {{if this.isCopied 'Copied!' 'Click to copy'}}
  <Icon @name={{'clipboard'}} />
</button>
```
<file-name>copy-to-clipboard.hbs</file-name>
<br />

</v-click>
<v-click>

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
<file-name>copy-to-clipboard.js</file-name>

</v-click>

<!-- 
Problem:
- where does Icon component come from: own app, addon, ...?
- where does the modifier come from: own, addon?
- Arrow function binds context (this)
  - saves two lines so code fits on my slides
 -->

---
layout: four-sections
---

::topleft::

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
<file-name>copy-to-clipboard.hbs</file-name>
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
<file-name>copy-to-clipboard.js</file-name>

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

<template>
  <button {{on 'click' this.copyToClipboard}}>
    {{if this.isCopied 'Copied!' 'Click to copy'}}
    <Icon @name={{'clipboard'}} />
  </button>
</template>
```
<file-name>copy-to-clipboard.glimmer</file-name>

</v-click>

<!-- 
Frontmatter
- smallest delta
- only imports, no other js
- hard for tooling to integrate split
- how to test?
- 
 -->

---
layout: four-sections
---

::topleft::

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

<file-name>copy-to-clipboard.js</file-name>

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

<file-name>copy-to-clipboard.gjs</file-name>

</v-click>

<!-- 
Template literals - tagged templates
- "The tag function can then perform whatever operations on these arguments you wish, and return the manipulated string"
- fairly unknown JS feature
- create custom identifier in front of template literal to run through a function
 -->

---

### Why a new component format?
# The path forward

- All solutions solve the **template import problem**
- Different trade-offs to be made in
  - Semantics
  - Learning
  - Tooling
  - Testing
- `<template>` tag components came out as best overall
- Since [RFC #779](https://rfcs.emberjs.com/id/0779-first-class-component-templates/) the accepted next-gen format


<span class="block absolute bottom-12 left-16 text-white/60 text-sm">
  See <a href="https://v5.chriskrycho.com/journal/ember-template-imports/">Ember Template Imports blog series</a> by Chris Krycho
</span>

---
layout: section
---

# Semantics of template tag components

<!--
Building a mental model
-->

---

### Semantics of template tag components
# How does it compare to the current format?

- Single file for both template and JS/TS
  - `.gjs` and `.gts` file extensions
  - Short for Glimmer JS and Glimmer TS
- Imports required for
  - Components
  - Helpers
  - Modifiers
- Wrapping glimmer template with the `<template>` tag

---
layout: four-sections
---

### Semantics of template tag components
# Template-only components

::topleft::

Before

```hbs
Hey <Icon @name={{"waving-hand"}} />
```
<file-name>greeting.hbs</file-name>

::topright::

<v-click>

After

```gjs{1,3,5}
import Icon from 'example-app/components/icon';

<template>
  Hey <Icon @name={{"waving-hand"}} />
</template>
```
<file-name>greeting.gjs</file-name>

<br />

<v-click>

Which is syntactic sugar for

```gjs{3}
import Icon from 'example-app/components/icon';

export default <template>
  Hey <Icon @name={{"waving-hand"}} />
</template>;
```
<file-name>greeting.gjs</file-name>
</v-click>

</v-click>

---
layout: four-sections
---

### Semantics of template tag components
# Class-based components

::topleft::

Before

```hbs
<button {{on 'click' this.copyToClipboard}}>
  {{if this.isCopied 'Copied!' 'Click to copy'}}
  <Icon @name={{'clipboard'}} />
</button>
```
<file-name>copy-to-clipboard.hbs</file-name>
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
<file-name>copy-to-clipboard.js</file-name>

::topright::

<v-click>

After

```gjs{3-4,14-19}
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
<file-name>copy-to-clipboard.gjs</file-name>

</v-click>

---
layout: statement
---

# But wait, there's more!

---
layout: four-sections
---

### Semantics of template tag components
# Scope access to any imported value


::topleft::

Before

```hbs{1-10|0}
Hello
{{#if (hasFeature this.FEATURE_WORLD)}}
  World
{{/if}}
```
<file-name>hello-world.hbs</file-name>
<br />

```js{0|1-10|0}
import { FEATURE_WORLD } from 'app/utils/feature-flags';

export default class HelloWorld extends Component {
  FEATURE_WORLD = FEATURE_WORLD;
}
```
<file-name>hello-world.js</file-name>

::topright::

<v-click>

After

```gjs
import hasFeature from 'app/helpers/has-feature';
import { FEATURE_WORLD } from 'app/utils/feature-flags';

<template>
  Hello
  {{#if (hasFeature FEATURE_WORLD)}}
    World
  {{/if}}
</template>
```
<file-name>hello-world.gjs</file-name>

</v-click>

<!-- 
- No need for backing class just to pass imports
- Refactoring now "just works"!
-->

---

### Semantics of template tag components
# Scope access to local values

```gjs
const value = 2;

const square = (number) => {
  return number * number;
};

<template>
  The square of {{value}} is {{(square value)}}
</template>
```

<!-- 
- Big win for defining local values that are inherent to the component
- Especially good for local modifiers, to replace ember-render-modifiers
-->

---

### Semantics of template tag components
# Also locally defined components!


```gjs
const CustomListItem = <template>
  <div class="p-4 rounded text-white bg-blue-700">
    {{yield}}
  </div>
</template>;

const CustomList = <template>
  {{#each @items as |item|}}
    <CustomListItem>
      {{item.value}}
    </CustomListItem>
  {{/each}}
</template>;

export default CustomList;
```


<!-- 
With great power comes great responsibility
- Split components
- Like you can split functions, lower barrier
- Seen components get hude overtime
- Don't overuse
 -->

---
layout: four-sections
---

### Semantics of template tag components
# Testing

::topleft::

Before

```gjs{3,9-12|0}
import { module, test } from 'qunit';
import { setupRenderingTest } from 'ember-qunit';
import { hbs } from 'ember-cli-htmlbars';

module('copy-to-clipboard', function (hooks) {
  setupRenderingTest(hooks);

  test('renders', async function (assert) {
    this.text = 'Hello EmberConf 2023!';
    await render(hbs`
      <CopyToClipboard @text={{this.text}} />
    `);

    assert.dom('[data-test-copy]').exists();
  });
});
```

::topright::

<v-click>

After

```gjs{10-14|3|9,12}
import { module, test } from 'qunit';
import { setupRenderingTest } from 'ember-qunit';
import CopyToClipboard from 'components/copy-to-clippy';

module('copy-to-clipboard', function (hooks) {
  setupRenderingTest(hooks);

  test('renders', async function (assert) {
    const text = 'Hello EmberConf 2023!';
    await render(
      <template>
        <CopyToClipboard @text={{text}} />
      </template>
    );

    assert.dom('[data-test-copy]').exists();
  });
});
```
</v-click>

<!-- 
- Same syntax for testing and writing components!
- No longer needed to assign state to `this`
 -->

---
layout: statement
---

# What about styling?

---

### Semantics of template tag components
# Styling in template tag components

- Template tag format is not opinionated about styling
- Everything "Just Works"
  - Separate files for (S)CSS
  - TailwindCSS
  - CSS modules

<br />

```gjs
<template>
  <style>
    .danger { color: red; }
  </style>
  <span class="danger">Watch out!</span>
</template>
```

<!--
`glimmer-scoped-css`

`embroider-css-modules`

`ember-scoped-css`
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
- https://github.com/emberjs/babel-plugin-ember-template-compilation
- RFC 931
 -->

---

### Using template tag components today
# Underlying implementation *will* change

- `ember-template-imports` is an exploration addon to use template tags today
- Content tag spec for generalizing embedding
  - Enables future design extensions, e.g. `<style>` for scoped CSS or `<query>` for GraphQL
  - Framework agnostic: other tools could reuse this format
- New `content-tag` package
  - Preprocessor for rewriting to valid JS
  - Written in Rust on top of Speedy Web Compiler (SWC)
- No syntactical impact for end consumer

<!-- 
to make it easy for tooling authors to know when they have implemented the support correctly, including answering questions about nested invocation, escaping, handling closing tags within comments, etc.

to enable future design moves in this space—for example, <style> blocks for scoped CSS or <query> blocks for languages like GraphQL, etc.

to enable other parts of the JS ecosystem to adopt this—in the same way that JSX is not a React-only syntax but a spec which can also be used by e.g. Solid—since it could be valuable authoring format for Svelte, Vue, and other template-focused/HTML-first frameworks
-->

---

### Using template tag components today
# Linting with ESLint

- `ember-template-lint` v5.8.0 or higher
- `eslint-plugin-ember` v11.6.0


---

### Using template tag components today

# Code formatting with Prettier

> **prettier-plugin-ember-template-tag** <br/>
> A Prettier plugin for formatting Ember template tags in both .gjs and .gts files

Install

```sh
$ pnpm add --save-dev prettier-plugin-ember-template-tag
```

Configure

```js
module.exports = {
  plugins: ['prettier-plugin-ember-template-tag'],
  overrides: [
    {
      files: '*.{js,ts,gjs,gts}',
    },
  ],
};
```
<file-name>.prettierrc.js</file-name>


---
layout: four-sections
---

### Using template tag components today

# Type checking in templates using Glint

- Glint provides end-to-end typechecking in templates
- Both environment configs `ember-loose` and `ember-template-imports` are required

Install

```sh
$ pnpm add --save-dev @glint/environment-ember-template-imports
```

Configure

::topleft::

```json
{
  "extends": "@tsconfig/ember/tsconfig.json",
  "glint": {
    "environment": [
      "ember-loose",
      "ember-template-imports"
    ]
  }
}
```
<file-name>tsconfig.json</file-name>

::topright::

```ts
import "@glint/environment-ember-loose";
import "@glint/environment-ember-template-imports";
```
<file-name>global.d.ts</file-name>

---

### Using template tag components today

# Code editor setup

VS Code

- [vscode-glimmer](https://marketplace.visualstudio.com/items?itemName=chiragpat.vscode-glimmer)
  - `chiragpat.vscode-glimmer`
- Registers `Glimmer-JS` and `Glimmer-TS` languages to VS Code
- Adds syntax highlighting

<br />
<br />

<v-click>

> **Using another code editor?**<br />
> Highly likely it's supported, multiple Grammar definitions are available on GitHub<br />
> Checkout the [ember-template-imports](https://github.com/ember-template-imports/ember-template-imports) GitHub repository

</v-click>

---
layout: four-sections
---

### Using template tag components today

# Syntax highlighting on GitHub/GitLab

- Not yet supported
  - Currently ~1100 `.gjs`/`.gts` files on GitHub of the required 2000
- Configurable to fall back to JS/TS syntax highlighting

<br/>
<br/>
<br/>
<br/>
<br/>

::topleft::

<logos-github-icon style="filter: brightness(0) invert(1);"/> GitHub

```
*.gjs linguist-language=js linguist-detectable
*.gts linguist-language=ts linguist-detectable
```
<file-name>.gitattributes</file-name>

::topright::

<logos-gitlab /> GitLab

```
*.gjs gitlab-language=js
*.gts gitlab-language=ts
```
<file-name>.gitattributes</file-name>

<!-- 
- 2000 `.gjs`/`.gts` component files required for GitHub to support
 -->

---

### Using template tag components today
# Using it in your browser

Interactive tutorial [https://tutorial.glimdown.com](https://tutorial.glimdown.com) by NullVoxPopuli

![Glimdown](/images/glimdown.png)


---
layout: section
---

# Looking ahead

---

### Looking ahead
# Usage in routes

- Currently not possible to use `.gjs`/`.gts` for route templates
- `setRouteComponent` RFC
- New router will unlock this

---

### Looking ahead
# Codemod

<Tweet id="1589671595039416320" conversation="1589671173834801152"/>


---

### Looking ahead
# Wishlist

- Automatic imports in template
- Ember CLI blueprints
- Documentation

---
layout: section
---

# Wrapping it up

---

### Wrapping it up
# Current state of template tag components

- Officially the **next-gen component format** for Ember.js
- Already usable **today**
- Can be adopted **incrementally**
- Allows access to **local scope** in templates
- Offers a **streamlined testing experience**
- Better tooling compatibility increases **developer experience**
- Unlocks **performance optimizations** for bundlers


---
layout: statement
---

# Should you start using &lt;template&gt; tag components today?

<!--
- If you can live with tooling not being complete, rough edges -> yes
- Otherwise okay to hold off a bit
  - At least you know it's coming now
- Personally: I'm super productive with template tags/TypeScript/Glint and it's my go-to choice when starting new apps
-->

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