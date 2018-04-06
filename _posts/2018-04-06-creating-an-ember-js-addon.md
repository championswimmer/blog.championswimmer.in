---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: Creating an Ember.js Addon
tags: 'emberjs, javascript, typescript'
categories:
  - webdevelopment
description: Creating Ember.js Addon with Typescript
---
## Getting Started
Use ember-cli to create basic structure (like you start with creating an app) 

```shell 
ember addon <addon-name>
```

This will generate the following directory structure  

```
.
├── addon					# your addon code goes here
├── app						# things you want to inject into consumer apps' namespace 
├── config					
│   ├── ember-try.js			# testing config
│   └── environment.js
├── ember-cli-build.js		# how the dummy test app is built
├── index.js					# how your app is built (read more - https://ember-cli.com/api/classes/Addon.html ) 
├── package-lock.json
├── package.json
├── testem.js
├── tests
│   ├── dummy				# dummy app (for testing) 
│   │   ├── app
│   │   ├── config
│   │   └── public
│   ├── helpers
│   ├── index.html
│   ├── integration			# e2e tests for your addon
│   ├── test-helper.js		# test setup config for your addon
│   └── unit					# unit tests for your addon
└── vendor
```


## Adding an util

```shell
ember generate util <util-name>
```


## Using Typescript

### `extend` instead of `.create()`

In JS we write 

```javascript
export default Mixin.create({})
```

We can instead use - 
```typescript
export default class PromiseResolver extends Mixin<PromiseResolver> {}
```


> Written with [StackEdit](https://stackedit.io/).
