---
title: "API: The router Property"
description: The router property lets you customize Nuxt.js router.
---

> The router property lets you customize Nuxt.js router ([vue-router](https://router.vuejs.org/en/)).

## base

- Type: `String`
- Default: `'/'`

The base URL of the app. For example, if the entire single page application is served under `/app/`, then base should use the value `'/app/'`.

This can be useful if you need to serve Nuxt as a different context root, from within a bigger Web site. Notice that you may, or may not set up a Front Proxy Web Server.

If you want to have a redirect to `router.base`, you can do so [using a Hook, see *Redirect to router.base when not on root*](/api/configuration-hooks#redirect-to-router-base-when-not-on-root).

Example (`nuxt.config.js`):
```js
export default {
  router: {
    base: '/app/'
  }
}
```

<div class="Alert Alert-blue">

When `base` is set, Nuxt.js will also add in the document header `<base href="{{ router.base }}"/>`.

</div>

> This option is given directly to the vue-router [base](https://router.vuejs.org/api/#base).

## routeNameSplitter

- Type: `String`
- Default: `'-'`

You may want to change the separator between route names that Nuxt.js uses. You can do so via the `routeNameSplitter` option in your configuration file.
Imagine we have the page file `pages/posts/_id.vue`. Nuxt will generate the route name programatically, in this case `posts-id`. Changing the `routeNameSplitter` config to `/` the name will therefore change to `posts/id`.

Example (`nuxt.config.js`):
```js
export default {
  router: {
    routeNameSplitter: '/'
  }
}
```

## extendRoutes

- Type: `Function`

You may want to extend the routes created by Nuxt.js. You can do so via the `extendRoutes` option.

Example of adding a custom route:

`nuxt.config.js`
```js
export default {
  router: {
    extendRoutes (routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, 'pages/404.vue')
      })
    }
  }
}
```

The schema of the route should respect the [vue-router](https://router.vuejs.org/en/) schema.

<div class="Alert Alert--orange">

<b>Warning:</b> when adding routes that use [Named Views](/guide/routing#named-views), don't forget to add the corresponding `chunkNames` of named `components`.

</div>

`nuxt.config.js`
```js
export default {
  router: {
    extendRoutes (routes, resolve) {
      routes.push({
        path: '/users/:id',
        components: {
          default: resolve(__dirname, 'pages/users'), // or routes[index].component
          modal: resolve(__dirname, 'components/modal.vue')
        },
        chunkNames: {
          modal: 'components/modal'
        }
      })
    }
  }
}
```

## fallback

- Type: `boolean`
- Default: `false`

Controls whether the router should fallback to hash mode when the browser does not support history.pushState but mode is set to history.

Setting this to false essentially makes every router-link navigation a full page refresh in IE9. This is useful when the app is server-rendered and needs to work in IE9, because a hash mode URL does not work with SSR.

> This option is given directly to the vue-router [fallback](https://router.vuejs.org/api/#fallback).

## linkActiveClass

- Type: `String`
- Default: `'nuxt-link-active'`

Globally configure [`<nuxt-link>`](/api/components-nuxt-link) default active class.

Example (`nuxt.config.js`):

```js
export default {
  router: {
    linkActiveClass: 'active-link'
  }
}
```

> This option is given directly to the vue-router [linkactiveclass](https://router.vuejs.org/api/#linkactiveclass).

## linkExactActiveClass

- Type: `String`
- Default: `'nuxt-link-exact-active'`

Globally configure [`<nuxt-link>`](/api/components-nuxt-link) default exact active class.

Example (`nuxt.config.js`):

```js
export default {
  router: {
    linkExactActiveClass: 'exact-active-link'
  }
}
```

> This option is given directly to the vue-router [linkexactactiveclass](https://router.vuejs.org/api/#linkexactactiveclass).

## linkPrefetchedClass

- Type: `String`
- Default: `false`

Globally configure [`<nuxt-link>`](/api/components-nuxt-link) default prefetch class (feature disabled by default)

Example (`nuxt.config.js`):

```js
export default {
  router: {
    linkPrefetchedClass: 'nuxt-link-prefetched'
  }
}
```

## middleware

- Type: `String` or `Array`
  - Items: `String`

Set the default(s) middleware for every page of the application.

Example:

`nuxt.config.js`

```js
export default {
  router: {
    // Run the middleware/user-agent.js on every page
    middleware: 'user-agent'
  }
}
```

`middleware/user-agent.js`
```js
export default function (context) {
  // Add the userAgent property in the context (available in `asyncData` and `fetch`)
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```

To learn more about the middleware, see the [middleware guide](/guide/routing#middleware).

## mode

- Type: `String`
- Default: `'history'`

Configure the router mode, this is not recommended to change it due to server-side rendering.

Example (`nuxt.config.js`):

```js
export default {
  router: {
    mode: 'hash'
  }
}
```

> This option is given directly to the vue-router [mode](https://router.vuejs.org/api/#mode).

## parseQuery / stringifyQuery

- Type: `Function`

Provide custom query string parse / stringify functions. Overrides the default.

> This option is given directly to the vue-router [parseQuery / stringifyQuery](https://router.vuejs.org/api/#parsequery-stringifyquery).

## prefetchLinks

> Added with Nuxt v2.4.0

- Type: `Boolean`
- Default: `true`

Configure `<nuxt-link>` to prefetch the *code-splitted* page when detected within the viewport.
Requires [IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) to be supported (see [CanIUse](https://caniuse.com/#feat=intersectionobserver)).

We recommend conditionally polyfilling this feature with a service like [Polyfill.io](https://polyfill.io):

`nuxt.config.js`

```js
export default {
  head: {
    script: [
      { src: 'https://polyfill.io/v2/polyfill.min.js?features=IntersectionObserver', body: true }
    ]
  }
}
```

To disable the prefetching on a specific link, you can use the `no-prefetch` prop:

```html
<nuxt-link to="/about" no-prefetch>About page not pre-fetched</nuxt-link>
```

To disable the prefetching on all links, set the `prefetchLinks` to `false`:

```js
// nuxt.config.js
export default {
  router: {
    prefetchLinks: false
  }
}
```

## scrollBehavior

- Type: `Function`

The `scrollBehavior` option lets you define a custom behavior for the scroll position between the routes. This method is called every time a page is rendered. To learn more about it, see [vue-router scrollBehavior documentation](https://router.vuejs.org/guide/advanced/scroll-behavior.html).

<div class="Alert Alert-blue">

Starting from v2.9.0, you can use a file to overwrite the router scrollBehavior, this file should be placed in `~/app/router.scrollBehavior.js`.

</div>

You can see Nuxt default `router.scrollBehavior.js` file here: [packages/vue-app/template/router.scrollBehavior.js](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/router.scrollBehavior.js).

Example of forcing the scroll position to the top for every routes:

`app/router.scrollBehavior.js`
```js
export default function (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

## trailingSlash

- Type: `Boolean` or `undefined`
- Default: `undefined`
- Available since: v2.10

If this option iss set to true, trailing slashes will be appended to every route. If set to false, they'll be removed.

**Attention**: This option should not be set without preparation and has to be tested thoroughly. When setting `router.trailingSlash` to something else than `undefined`, the opposite route will stop working. Thus 301 redirects should be in place and you *internal linking* has to be adapted correctly. If you set `trailingSlash` to `true`, then only `example.com/abc/` will work but not `example.com/abc`. On false, it's vice-versa
