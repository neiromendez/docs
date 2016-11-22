---
title: Polymer CLI
---

<!-- toc -->

Polymer CLI is still pre-release. Some options may be subject to change.
{.alert .alert-warning}

## Install {#install}

1.  Install [Git](https://git-scm.com/downloads).

1.  Install the current LTS version (4.x) of
    [Node.js](https://nodejs.org/en/download/) or newer.

1.  Install the latest version of Bower.

        npm install -g bower

1.  Install Polymer CLI.

        npm install -g polymer-cli

You're all set. Run `polymer help` to view a list of commands.

## Overview {#overview}

Polymer CLI is a command-line interface for Polymer projects. It includes a build pipeline, a boilerplate generator for creating elements and apps, a linter, a development server, and a test runner.

Polymer CLI works with two types of projects:

* Elements projects. In an element project, you expose a single element or
  group of related elements which you intend to use
  in other element or app projects, or
  distribute on a registry like Bower or NPM. Elements are reusable and
  organized to be used alongside other elements, so components are referenced
  outside the project.
* Application projects. In an app project, you build an application, composed
  of Polymer elements, which you intend to deploy as a website. Applications are
  self-contained, organized with components inside the application.

Complete the [Install](#install) section to learn how to install
Polymer CLI and then proceed to the [Build an element](#element) or
[Build an app](#app) section to get started on your first project.

## Create an element project {#element}

This section shows you how to start an element project.

1.  Create a directory for your element project. It's best practice for the name
    of your project directory to match the name of your element.

    <pre><code>mkdir <var>my-el</var> && cd <var>my-el</var></code></pre>

    Where <code><var>my-el</var></code> is the name of the element you're
    creating.

1.  Initialize your element. Polymer CLI asks you a few
    questions as it sets up your element project.

        polymer init

1.  Select `element`.

1.  Enter a name for your element.

    The [custom elements
    specification](https://www.w3.org/TR/2016/WD-custom-elements-20160226/#concepts) requires the
    element name to contain a dash.
    {.alert .alert-info}

1.  Enter a description of the element.

At this point, Polymer CLI generates files and directories for your element,
and installs your project's dependencies.

### Element project layout

After the initialization process Polymer CLI generates the following files and directories.

*   `bower.json`. Configuration file for Bower.
*   `bower_components/`. Project dependencies. See
    [Manage dependencies](#dependencies).
*   `demo/index.html`. Demo of <code><var>my-el</var></code>`.html`.
*   `index.html`. Automatically-generated API reference.
*   <code><var>my-el</var></code>`.html`. Element source code.
*   `test/`<code><var>my-el</var></code>`_test.html`. Unit tests for
    the element.

#### HTML imports and dependency management {#element-imports}

Summary:

When importing Polymer elements via HTML imports, always use the relative URL
<code>../<var>package-name</var>/<var>element-name</var>.html</code> (e.g.
`../polymer/polymer.html` or `../paper-elements/paper-button.html`).

Details:

Suppose that you ran Polymer CLI to generate an element project. Your element is named `my-el`. You look inside `my-el.html` and see that Polymer has been
imported like so:

    <link rel="import" href="../polymer/polymer.html">

This path doesn't make sense. Relative to `my-el.html`,
Polymer is actually located at `bower_components/polymer/polymer.html`. Whereas
the HTML import above is referencing a location *outside* of your element
project. What's going on?

Bower installs dependencies in a flat dependency tree, like so:

    bower_components/
      polymer/
        polymer.html
      my-el/
        my-el.html
      other-el/
        other-el.html

This works well on the application-level. All elements are siblings, so they can
all reliably import each other using relative paths like
`../polymer/polymer.html`. This is why Polymer CLI uses relative paths
when initializing your element project.

However, one problem with this approach, as stated earlier, is that this
structure does not actually match the layout in your element project. Your
element project is actually laid out like so:

    bower_components/
      polymer/
        polymer.html
    my-el.html

Polymer CLI handles this by remapping paths. When you run `polymer serve`,
all elements in `bower_components` are remapped to appear to be in sibling
directories relative to `my-el`. The current element is served from the
made-up path of <code>/components/<var>bower name</var></code>, where
<code><var>bower name</var></code> is the `name` field from your element
project's `bower.json` file.

## Create an app project {#app}

Polymer CLI supports initializing a project folder with one of
several application templates.  The CLI comes with a `basic` template,
which is the most basic starting point for a Polymer-based application,
as well as others that introduce more complex layout and application patterns.

This chapter teaches you more about `basic` app projects.  See
[Polymer App Toolbox templates](../../toolbox/templates) for more details on
other templates.

### App project architecture {#app-architecture}

Polymer CLI is designed for apps that follow the [app shell
architecture](https://developers.google.com/web/updates/2015/11/app-shell).

There are fundamental concepts of the app shell architecture that you should understand before creating your app project with Polymer CLI: the entrypoint,
the shell, and fragments. See [App structure](/1.0/toolbox/server#app-structure)
from the App Toolbox docs for an in-depth overview of these concepts.

### Set up basic app project {#basic-app}

Follow the steps below to get your `basic` app project set up.

1.  Create a directory for your app project.

    <pre><code>mkdir <var>app</var>
    cd <var>app</var></code></pre>

    Where <code><var>app</var></code> is the name of your project directory.

1.  Initialize your app. Polymer CLI asks you a few questions
    as it sets up your app.

        polymer init

1.  Select `application`.

1.  Enter a name for your app. Defaults to the name of the current directory.

1.  Enter a name for the main element in your project. The main element is the
    top-most, application-level element of your app. Defaults to the name of
    the current directory, followed by `-app`.

    The code samples throughout this doc use the example app element name
    <code><var>my-app</var></code>. When creating your app you'll want to
    replace any instance of <code><var>my-app</var></code> with the name of
    your main element.
    {.alert .alert-info}

1.  Enter a description for your app.

At this point, Polymer CLI generates files and directories for your app,
and installs your project's dependencies.

### App project layout

After the initialization process Polymer CLI generates the following files and directories.

*   `bower.json`. Configuration file for Bower.
*   `bower_components/`. Project dependencies. See
    [Manage dependencies](#dependencies).
*   `index.html`. Entrypoint page of the app.
*   `src/`<code><var>my-app</var></code>/<code><var>my-app</var></code>`.html`.
    Source code for main element.
*    `test/`<code><var>my-app</var></code>/<code><var>my-app</var></code>`_test.html`. Tests for main element.

#### Add elements

You may want to compose your main element out of smaller, application-specific
elements. These app-specific elements should be defined in the `src` directory, at the same level as <code><var>my-app</var></code>.

    app/
      src/
        my-app/
        my-el/

Currently there is no Polymer CLI command to generate application-specific elements. You should do it by hand and should not create an [element project](#element) within your app project.

## Commands {#commands}

This section explains various useful Polymer CLI commands that you'll want to incorporate into your development workflow while you build your element or app project.

The commands are intended for both element and app projects unless otherwise
noted.

### Run tests {#tests}

If you want to run tests on your element or app project, `cd` to the base directory of your project and run:

    polymer test

Polymer CLI automatically runs all of the tests that it finds in the `test` directory. You'll see the results of the tests in your CLI.

If you create your own tests, they should also go in the `test` directory.

The underlying library that powers `polymer test` is called `web-component-tester` (`wct`). Learn more about creating unit tests with `wct`
in [Test your elements](/1.0/tools/tests).

### Run local web server {#serve}

If you want to view a live demo of your element or app, run the local web server:

    polymer serve

To view the demo, point your browser to one of the following URLs.

Element project demo:

<pre><code>http://localhost:8080/components/<var>my-el</var>/demo/</code></pre>

Element project API reference:

<pre><code>localhost:8080/components/<var>my-el</var>/</code></pre>

App project demo:

    http://localhost:8080

#### Server options {#server-options}

This section shows examples of using various `polymer serve` options.

Serve from port 3000:

    polymer serve --port 3000

If you have configured a custom hostname on your machine, Polymer CLI can serve it with the
`--hostname` argument (for example, app project demo is available at `http://test:8080`):

    polymer serve --hostname test

Open up a page other than the default `index.html` in a specific browser
(Apple Safari, in this case):

    polymer serve --open app.html --browser Safari

### Lint element(s) {#lint}

Check elements in your element project or app project for syntax errors, anti-patterns and more.

Element project example:

    polymer lint --input my-el.html

App project example (linting multiple elements):

    polymer lint --root src/ --input my-app/my-app.html my-el/my-el.html

If all of the elements you want to test are in the same directory, you can specify the `--root` flag to make all of the `--input` files relative to that directory.

### Build app {#build}

*This command is for app projects only.*

Generates a production-ready build of your app. This process includes minifying the HTML, CSS, and JS of the application dependencies, and generating a service worker to pre-cache dependencies.

Polymer CLI's build process is designed for apps that follow the [app shell architecture](https://developers.google.com/web/updates/2015/11/app-shell). To make sure your app builds properly, create a `polymer.json` file at the top-level of your project and store your
build configurations there. The following properties can be used:

- `entrypoint`: The main entrypoint into your application for all routes. Often times this is your `index.html` file. This file should import the app shell file specified in the shell option. It should be minimal since it's loaded and cached for each route.
- `shell`: The app shell file containing common code for the app.
- `fragment`: An array of any HTML files that are not synchronously loaded from the app shell, such as async imports or any imports loaded on-demand (e.g. by importHref).
- `sources`: An optional array of globs matching your application source files. This will default to all files in your project `src/` directory, but configuring your own list of sources can be useful when your source files live in other directories.
- `includeDependencies`: An optional array of globs matching any additional dependencies you'd like to include with your build. If your application loads any files dynamically they can be missed by the analyzer, but you can include them here to make sure that they are always added to your build.
  *Note: If you ever use polymer-build to define your own build process you can decide to handle sources & dependencies differently. But Polymer CLI currently treats additional files included in `sources` & `includeDependencies` the same, so place any additional files wherever you think makes the most sense.

For example, suppose you added an app shell (`app-shell.html`) and two views (`view-one.html` and `view-two.html`) for your `basic` app project, as well as a directory of images to display within your application. You'd specify them in your build with the following `polymer.json` configuration:

```json
{
  "entrypoint": "index.html",
  "shell": "src/app-shell/app-shell.html",
  "fragments": [
    "src/view-one/view-one.html",
    "src/view-one/view-two.html"
  ],
  "sources": [
    "src/**/*",
    "images/**/*",
    "bower.json"
  ],
  "includeDependencies": [
    "bower_components/webcomponentsjs/webcomponents-lite.min.js"
  ]
}
```

You can also pass these values via command-line flags. For example, in a newly created `basic` app project you could run the following command to generate a build:

    polymer build --entrypoint index.html

This can be useful for building simple projects on your machine but you will need to include the flag every time you run the command. For most projects a `polymer.json` configuration file will be easier to work with and share across your team.

#### Service workers

Polymer CLI will generate a service worker for your build using the [sw-precache](https://github.com/GoogleChrome/sw-precache) library. To customize your service worker, create a `sw-precache-config.js` file in your project directory that exports your configuration. See the [sw-precache README](https://github.com/GoogleChrome/sw-precache) for a list of all supported options.

Note that the sw-precache library uses a cache-first strategy for maximum speed and makes some other assumptions about how your service worker should behave. Read the ["Considerations"](https://github.com/GoogleChrome/sw-precache#considerations) section of the sw-precache README to make sure that this is suitable for your application.

#### Bundled and unbundled builds {#bundles}

Polymer CLI generates two build versions:

*   `bundled`. All fragments are bundled together to reduce the number of file
    requests. Optimal for sending to clients or serving from servers that are
    not HTTP/2 compatible.
*   `unbundled`. Fragments are unbundled. Optimal for HTTP/2-compatible servers
    and clients.

## Manage dependencies {#dependencies}

Polymer CLI uses [Bower](http://bower.io) for dependency management.

Dependencies are stored in the `bower_components` directory. You should never manually alter the contents of this directory.

Use the Bower CLI to manage dependencies.

```bash
# downloads dependency to bower_components/
# --save flag saves new dependency to bower.json
bower install --save PolymerElements/iron-ajax
# removes dependency from bower_components/ and bower.json
bower uninstall PolymerElements/iron-ajax
```

## Global options {#global-options}

You can see a list of global options by running `polymer help`. Most of them
are self-explanatory.

The following commands are currently only supported for the `polymer build`
command, with planned support for other commands in the future.

*   `entry`
*   `shell`
*   `fragment`

See [Build app](#build) for more information on how to use these options. 

Deploying the app via GitHub Pages

You can deploy your apps quickly via:

polymer github-pages:deploy --message "Optional commit message"
This will do the following:

creates GitHub repo for the current project if one doesn't exist

creates a local gh-pages branch if one doesn't exist

moves your app to the gh-pages branch and creates a commit

edit the base tag in index.html to support github pages

pushes the gh-pages branch to github

Creating the repo requires a token from github, and the remaining functionality relies on ssh authentication for all git operations that communicate with github.com. To simplify the authentication, be sure to setup your ssh keys.

