## Electron Platform: The Manifesto

### The single paragraph elevator pitch

The Electron Platform provides all of the commands that you need to run, test, and distribute Electron applications, so that your repo only needs to contain your code and the metadata necessary to build a production-ready solution on all of Electron's supported platforms. Electron applications consist of *just your code*, and doesn't prescribe a build system or inject a ton of boilerplate code.

### Red threads

* Electron apps are just node.js packages with some special fields in package.json and some specific folders that we look for. This means that making them compatible with other platforms like Chrome Apps or Cordova isn't a constant fight (though compatibility won't be an explicit goal, we just won't make it a PITA by adding lots of custom build crap).

* This means that default Electron apps don't use Grunt, Gulp, or anything else, though they can if a developer wants to add it in later.

* Even though they're built for production, Electron apps are super easy to get started with - the distance between "npm install" and "I have some text on the screen and DevTools open" should be as short as possible.

* Electron apps can be written in JavaScript ES6 (or ES5), TypeScript, or CoffeeScript. All three languages get dynamically compiled at runtime, but are precompiled when apps are packaged.

* (maybe, maybe not) - Electron apps have Bower pre-setup and configured so it'll work well in Electron. Adding CSS / front-end libraries should be super easy (though we should encourage apps to bring in JavaScript via `require`, not by using script tags).

### Commands / Scenarios in order of importance

* `new` - creates the template below and runs `npm install` (via Yeoman instead?)
* `run` - run the application on the current OS, in Dev Mode (i.e. everything is dynamically compiled). Takes a `--test` flag that will pop an interactive test window
* `test` - run specs then quit (for CI build)
* `lint` - compile as if we're packaging, but stop before we actually generate a new package.
* `package` - generate a production-level package for the current OS (i.e. on Windows generate a Squirrel release directory, on OS X generate a zip file with a signed .app in it, etc). Runs all precompilation steps.

### Template Application Structure

```
.jshintrc

window/           ## Renderer Process
  .jshintrc
  main.css
  main.html
  main.js
  components/     ## Bower

test/
  .jshintrc
  example-test.js

main.js           ## Browser Process
menus.json
package.json

build/
  authenticode.p12
  osx-cert.p12
  installer.ico
  app.ico
  app.icns
  app.plist

node_modules/
  electron-cli
    electron-prebuilt
    electron-compile  ## Does compilation and linting for electron-cli
  electron-menus      ## window + menus.json => event emitting
  electron-compile    ## Sets up Babel + TypeScript + CoffeeScript
  electron-specs      ## Powers unit test stuff
```

A few notes about the structure of this template:

* This structure attempts to make it super clear about the "Main process" (aka what we call the Browser process today) and "Window Processes" (what we call the Renderer process today). This has traditionally been something that new developers have a hard time grokking, and having explicit structure here will help developers get it right.

* While this still looks like a lot of crap under `build`, it's the minimum amount of crap I can get it down to for a production app that runs on all three major platforms. As simple as possible, but no simpler - we shouldn't create a system that can't actually be _used_ once an app becomes something Srs Bzns.

* Anything under `node_modules` is especially up for change, I just tried to sketch something out.
