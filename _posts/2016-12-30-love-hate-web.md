---
title: Love-Hate Web Devlopment Toolchain
---

Web application development has the potential to mercilessly drown all of your
creative spark and inundate you in so much of tooling that you'll be wishing
for pointers in C.

<!--more-->

Developing for the web has its perks but a sane development
environment is certainly not one of them. I just spent about 3 hours setting
up my environment for TypeScript with RxJS. Three goddamn hours of life wasted
configuring Vim, the TypeScript compiler, and the SystemJS module loader.
You could argue that I learned various module options provided by the compiler or that I
invented a nifty trick to navigate the plugin scripts, but I didn't want to
do that. I wanted to relish the functional beauty of RxJS and instead
I am ranting about the mess that my dev environment is in. Let me tell
you what feats of acrobatics one has to perform to get their dev environment
in order.

## How I Fumbled Around

1. Started by investigating why YouCompleteMe wasn't kind enough to complete
  properties on my `Observable` object which I had just imported.
2. Turns out that YouCompleteMe had nothing against me. My import statement was
  wrong. But Syntastic is supposed to watch my back right?
3. `tslint` works with Syntastic but not `tsc`, why?
4. Why?
5. Why are you being so cruel to me oh Syntastic?
6. Fuck. The support for `tsc` is dropped! What the hell! I just saw people
  posting their debug info with working `tsc` on the GitHub issues.
7. Anyway, let's get on with RxJS.
8. Compile and load the built script on `index.html`. `define` not defined
  error in the browser. What the heck?!
9. Oh, you need a darn module loader. Okay. Let's use SystemJS.
10. Unlike RequireJS, SystemJS isn't considerate enough to apply the `.js`
  extension. Use the `package` config setting.
11. No errors in the browser! Yay! But no `Hello World` in the console.
12. Import the `main.js` file.

You see too many fucking steps.

## Vim

I love Vim. I can record a macro which calculates an arithmetic operation
using the expression register, save that macro, and apply to it all the lines
containing _vim is awesome_, but I sometimes hate Vim from the bottom of my
heart. It has no support for projects: a functionality you simply can't live
without in the modern age of software development. Sure there are plugins to
do that shit, but you have to download that plugin, set it up, configure it,
and somehow get it running. Couple this with the abysmal support for semantic
completion and you start wondering whether all the saved mental effort of text
editing is really worth it. For me it is (I have a dysfunctional relationship
with my editor). The fact that all of your editing environment can be
programmed so seamlessly is the biggest win for me and trumps everything else.
I've spent a lot of time getting syntax highlighting, file navigation,
autocompletion, and git integration right (what a shame that Fugitive doesn't
work properly on windows) for Vim. Things that any other modern code editor
gets right without having to install any goddamn plugin.

I understand that this de-centralized development in the wild is what has
allowed for some of the best plugins and code editing innovation (I bow to thee
Tim Pope), but configuring it can be a huge pain. I could live with this
requirement of knowing your platform before consuming it, but these days I am
expected to dispose of the old languages and my carefully crafted editor set
up with it like it was tissue paper. Everybody wants to wipe their asses
with some new programming language and framework out there. Is this modicum
of stability too much to ask for?

## Fucking Build Systems of Web

If there is one thing capable of turning me into a fire-spitting dragon gone
mad with rage, then it has to be the range of build systems in web
application development. It is a big motherfucking nuisance. Grunt, Gulp,
Broccoli, Brunch, etc. Don't people get tired of writing this shit? I am
all lost for words on this one.

## JavaScript

Again, my relationship with JavaScript is somewhat dysfunctional. I love it
as a scripting language, but ask me to write a full-fledged web application
of moderate complexity and I'll vanish into the thin air. Enough has been
said about how bad JavaScript is, so there doesn't seem to be much point
in lambasting it anymore. Although it is getting some major updates in the
upcoming specifications, you still have to setup Babel to get the module
system working. This, again, is an example of unnecessary acrobatics
required to get the basics working. Modules are so fundamental to a language
that requiring a transpiler is greatly hinders my ability to use it
effectively. For all of these reasons, I've decided to avoid pure JS as much
as possible and use proper TypeScript anywhere I can. I would have preferred
a drastic move to something like Clojure, but the community adoption and
library support go against it.

TypeScript is a pure pleasure to write. The way interfaces and object literals
interop in it is just pure gold. I would use TypeScript for this one additional
feature alone that it provides on top of ES6. As if this wasn't enough, the type
system powers a great semantic completion engine. Writing TypeScript in VSCode
or Visual Studio is plain joyful. Even the support for completion in Vim is
great, although not as great as the aforementioned IDEs. TypeScript just made
sense to me when I encountered it the first time. It was love at first sight,
but unlike JavaScript, which is more like a stormy passionate teenage love,
being with TypeScript is soothing. I hope that I don't end up hating it or
Microsoft or leave it.

