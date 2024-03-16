# Chrome Extension v3 in CLJS

## Problem

Can't get ClojureScript hot reloading to work when developing a Chrome Extension with shadow-cljs.

Details below.

## What have I tried so far

### 1

Tried to use the :browser and :esm targets in one of my projects, but it didn't work.

Figured it could be because of other elements of the project, so thought about trying in a "minimum" repo (next).

### 2

Cloned [Thomas Heller Chrome Extension example repo](https://github.com/thheller/chrome-ext-v3) here and tried it as is. Still didn't work.

### 3

Found the tip below [in the documentation](https://shadow-cljs.github.io/docs/UsersGuide.html#:~:text=If%20neither%20%3Aafter%2Dload%20nor%20%3Abefore%2Dload%20are%20set%20the%20compiler%20will%20only%20attempt%20to%20hot%20reload%20the%20code%20in%20the%20%3Abrowser%20target.%20If%20you%20still%20want%20hot%20reloading%20but%20don%E2%80%99t%20need%20any%20of%20the%20callbacks%20you%20can%20set%20%3Aautoload%20true%20instead.):

> If neither :after-load nor :before-load are set the compiler will only attempt to hot reload the code in the :browser target. If you still want hot reloading but don’t need any of the callbacks you can set :autoload true instead.

Since here we are using the `:target :esm`, seemed like this would fix the problem, but it didn't.

### 4

All this time I kind of knew it was a long shot, since [Chrome Extensions's manifest v3 is "anti-eval"](https://developer.chrome.com/docs/extensions/develop/migrate/improve-security) and eval seems essential for hot reloading.

Chrome shows a CSP (content security policy) error in the browser "Issues" tab, pointing to `cljs_env.js`, saying the following and pointing to [more csp documentation](https://web.dev/articles/csp?utm_source=devtools#eval_too):

> The Content Security Policy (CSP) prevents the evaluation of arbitrary strings as JavaScript to make it more difficult for an attacker to inject unathorized code on your site.
>
> To solve this issue, avoid using eval(), new Function(), setTimeout([string], ...) and setInterval([string], ...) for evaluating strings.
>
> If you absolutely must: you can enable string evaluation by adding unsafe-eval as an allowed source in a script-src directive.
