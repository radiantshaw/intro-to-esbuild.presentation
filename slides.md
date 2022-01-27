---
theme: slidev-theme-kiprosh
---

# ESBuild: An Introduction

---

# Why ESBuild?

<v-click>

![](/images/speed.svg)

</v-click>

<style>

img {
  margin-top: 12%;
}

</style>

---

# Installation

<v-clicks>

```bash
$ npm install esbuild
```

... installs `esbuild` inside the local `node_modules/` folder.

And, it also installs an `esbuild` executable inside the `node_modules/.bin/` folder

```bash
$ ./node_modules/.bin/esbuild --version
0.14.10
```

</v-clicks>

---

# Bundling

<v-click>

```bash
$ npm install react react-dom
```

</v-click>

<v-click>

Then we create an `src/app.jsx` file:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

function App() {
  return <p>Hello world!</p>;
}

ReactDOM.render(<App />, document.getElementById('root'));
```

</v-click>

<v-click>

... and then we build it:

```bash
$ ./node_modules/.bin/esbuild src/app.jsx --bundle --outfile=dist/out.js
```

</v-click>

---

# Using NPM Scripts

<v-click>

Move the command inside `package.json`:

```json
{
  "scripts": {
    "build": "esbuild src/app.jsx --bundle --outfile=dist/out.jsx"
  }
}
```

</v-click>

<v-click>

... which can then be used as:

```bash
$ npm run build
```

</v-click>

---

# Using A Build Script

<v-click>

Create a `scripts/build.js` script:

```js {1,5|2|3|4|all}
require('esbuild').build({
  entryPoints: ['src/app.jsx'],
  bundle: true,
  outfile: 'dist/out.js'
}).catch(() => process.exit(1));
```

</v-click>

<v-click>

Update the NPM script:

```json
{
  "scripts": {
    "build": "node scripts/build.js"
  }
}
```

</v-click>

---

# Ignore External Dependencies

<v-click>

```js {4}
require('esbuild').build({
  entryPoints: ['src/app.jsx'],
  bundle: true,
  external: ['react', 'react-dom'],
  outfile: 'dist/out.js'
}).catch(() => process.exit(1));
```

</v-click>

---

# Features

<style>

.slidev-layout h1 {
  font-size: 4.75rem;
  margin-top: 25%;
  text-align: center;
}

</style>

---

# TypeScript and JSX Support

<v-click>

```js
// TODO: Nothing!
```

... because the JSX loader is enabled for `.jsx`, and `.tsx` files.

</v-click>

<v-click>

But, for supporting JSX inside `.js` and `.ts` files:

```js {4-7}
require('esbuild').build({
  entryPoints: ['src/app.jsx'],
  bundle: true,
  loader: {
    '.js': 'jsx',
    '.ts': 'tsx'
  },
  outfile: 'dist/out.js'
}).catch(() => process.exit(1));
```

</v-click>

<v-clicks>

- **No Type Checking** is done so it has to be done manually

</v-clicks>

---

# Deno Support

<v-click>

```js {1|3-7|9|all}
import * as esbuild from 'https://deno.land/x/esbuild@v0.14.12/mod.js';

await esbuild.build({
  entryPoints: ['src/app.jsx'],
  bundle: true,
  outfile: 'dist/out.js'
});

esbuild.stop();
```

</v-click>

---

# Tree Shaking

<v-click>

Let's say we have a file `src/lib.js`:

```js
export function one() { console.log('one') }
export function two() { console.log('two') }
```

</v-click>

<v-click>

... and we `import` it inside our `src/app.js`:

```js
import * as lib from './lib.js';

lib.one();
```

</v-click>

<v-click>

It gets bundled as:

```js
function one() { console.log("one") }

one();
```

</v-click>

---

# Inbuilt Development Server

<v-click>

```js {1,7|1-3|3-7|all}
require('esbuild').serve({
  servedir: 'public'
}, {
  entryPoints: ['src/app.jsx'],
  bundle: true,
  outdir: 'public/dist'
});
```

</v-click>

---

# Any Questions?

<style>

.slidev-layout h1 {
  font-size: 4.75rem;
  margin-top: 25%;
  text-align: center;
}

</style>
