> Demo Vue bug where relative `srcset` URLs are not transformed

It looks like `@vue/compiler-sfc` has a bug, introduced in `v3.0.0-beta.9` when [adding support for absolute URLs in `srcset`](https://github.com/vuejs/vue-next/commit/6a0be882d4ce95eb8d8093f273ea0e868acfcd24). This bug bypasses the [transformation that would've resolved the asset URLs in `srcset`](https://github.com/vuejs/vue-next/blob/5eb72630a53a8dd82c2b8a9705c21a8075161a3d/packages/compiler-sfc/src/templateTransformSrcset.ts#L98-L158).

## Steps to reproduce:

1. In an SFC template, add an `<img>` tag with a `srcset` attribute, containing relative URLs with path aliases.

```html
<img srcset="@/assets/300.png 2x, @/assets/150.png 1x">
```

2. Start the dev server with `yarn dev`.

3. Observe no image is shown in the browser, and that the `<img>.srcset` URLs are unresolved.


```html
<!-- expected -->
<img srcset="/src/assets/300.png 2x, /src/assets/150.png 1x">

<!-- actual -->
<img srcset="/src/@/assets/300.png 2x, /src/@/assets/150.png 1x">
```