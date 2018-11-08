---
title: 'Better Caching on CI'
date: 2018-11-08
permalink: /posts/2018/11/better-caching-on-ci
tags:
   - Development
---

Build duration is annoyingly slow?
Welcome to caching. In our .gitlab-ci.yaml it's just a matter of

    cache:
      untracked: true
      paths:
        - node_modules/

and your next build will be sped up. Super cool, isn't it?


Pitfalls
--------

npm's way to manage dependencies isn't always deterministic and sometimes just do really weird. Especially if you mix versions like is it building on Node 8? Node 10? Node 11? In Gitlab-CI you just use different node-containers, they all come with different npm versions pre-packaged.
Would you like to manage different caches for it? Gitlab-CI does support that. Welcome to another dependency-maintenance hell.

What about upgrading some modules in a branch? Next time building `master` will it extract the newer version of that upgraded module? Will npm downgrade it? Are you really sure?
Of course you can use _package-lock.json_. Good luck, tell me if you weren't unhappy with it.

How I do caching in Gitlab-CI
-----------------------------

Since my days using composer on PHP projects, it's not a good advice to cache _vendor/_. Even if the official Gitlab-CI documentation tells you to cache _node_modules/_ and _vendor/_.
It might be a different machine, different bindings, missing post-install scripts, version-mismatch or other.

Still I need performance, so what to cache? Did you know, npm has it's own cache? By defaults it's somewhere at _~/.npm/_ (Linux). A simple command will show you:
`npm cache verify`

You can override it with an environment variable (or setting it in _~/.npmrc_). So my prefered way is to override npm's cache and cache that folder in gitlab-ci-runners.
Please note that I'm using a relative path, since gitlab-ci-runners can only cache relative paths (not absolute ones).

    cache:
      untracked: true
      paths:
        - .npm_cache/

    variables:
      NPM_CONFIG_CACHE: ".npm_cache"

We can even do it better by telling npm to prefer that cache (it will still take a look at the npm registry, but prefers the downloaded file anyway):
`npm install --prefer-offline`


Happy Caching!
