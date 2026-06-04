# vite-lib-build-issue-css-layer-order

Demonstrates the CSS Layer ordering issue when building with Vite 8+

## Instructions

- Install a new vuetify project using the [Installation](https://vuetifyjs.com/en/getting-started/installation/#installation) (and the package manager of your choice)
 - use default choices when prompted with questions
- Run `cd vuetify-project`
- Delete `vite.config.mts`
- Copy the [vite.config.ts](https://raw.githubusercontent.com/adube/vite-lib-build-issue-css-layer-order/refs/heads/main/vite.config.ts) file to the root of the project
- Run `npm run build-only`
- Copy the [mybuild.html](https://raw.githubusercontent.com/adube/vite-lib-build-issue-css-layer-order/refs/heads/main/mybuild.html) file to the `./dist/` directory
- Run `npx vite preview --outDir dist`
- Visit http://127.0.0.1:4173/mybuild.html

In another terminal:

- Run `npm run dev`
- Visit http://localhost:3000/

The page that uses the "lib" build, i.e. `mybuild.html` does not show up with the exact same appearance as the one served by vite (port 3000). If you inspect any of the `<v-card>` element, in the developer tool of your browser you'll see that that the CSS Layer are not in the same order in both pages.

## Fixed

Vuetify seems to inject `<style id="vuetify-theme-stylesheet" type="text/css">` tag at the end of the `<head>` tag. In dev, Vite injects styles in the head tag so those have the priority. But in `mybuild.html` the `<link rel="stylesheet" href="mybuild.css" />` is in `<body>`. This makes `<style id="vuetify-theme-stylesheet" type="text/css">` to have priority over `mybuild.css`.

So, the fix is: move `mybuild.css` to `<head>`.
