# Goal

We'd like to be able to **generate new OeX Platform plugins easily**.

## Why?

> Why just not to use ["official" plugins cookiecutter][edunext-cookiecutter] or its [RG's fork][rg-cookiecutter]?

- At this time we'd like to use in our customizations a progressive frontend app(s) - not just Django/Mako templates, but a [React]/[Redux] application (or a couple of such applications per plugin).
- React application itself is a separate codebase project.
- We'd like to keep the Plugin codebase and its Frontend(s) codebase(s) closely to each other and [Nx] manages such *monorepos* easily.

## What must be resolved

### New plugin initialization

> "Classic" plugin - the one that is generated by the Cookiecutter (a pluggable OeX Platform app with a classic template-based frontend).

There must be a way easily generate a "classic" OeX Platform plugin's code (with our enhancements: tests, CI, etc.).

### Optional progressive frontend initialization

> "Progressive" plugin - the one that will be generated by the Nx (a pluggable OeX Platform app that has an API for a comprehensive React-based frontend application).

There must be a way easily generate a "progressive" frontend application on the top of a "classic" OeX Platform plugin.

There must be a way to add more than one such progressive frontend.

### Plugin packaging

All monorepo's codebase must be able to be built into the single installable python package for the following distribution.

## Vision

"Progressive" monorepo includes the following parts:

- oex-plugin (python > django > DRF + ...)
- frontend-spa#1 (js > React + Redux + ...)
- frontend-spa#2 (js > React + Redux + ...)

### Open edX Platform plugin

Modifies or extends OeX Platform functionality.
It is installed to LMS or/and CMS as a dependency.
It exposes an API for its frontend application(s).
It uses built frontend application(s) as its "static".

> There must be a custom Nx plugin for the python/Django part generation.

### Frontend SPA

It modifies, substitutes, or extends OeX Platform's pieces of UI.
It is built and put inside the OeX Platform plugin.

> There must be a custom Nx plugin for the React/Redux part generation.

Options:

- add routing
- add forms

### Usage

> There must be implemented a custom `edx-plugin` [workspace generator][nx-ws-generator].

Workspace generator allows an initialization of a new OeX Platform plugin with an optional progressive frontend SPA + its API.

```bash
# generates a simple "classic" plugin:
nx workspace-generator edx-plugin

# generates a "progressive" plugin:
nx workspace-generator edx-plugin --preset=react

# generates another frontend SPA next to already existed:
nx generate @rg/react:app my-new-app
```

## Glossary

- `workspace` - a set of projects
- `preset` - an option for a new workspace
- `project`
- `app` - uses libs, implements features
- `lib` - common code (UI components, utils, etc.)
- `target`
- `plugin` - extends workspace capabilities (includes generators and executors)
- `generator` - automate making changes to the file system
- `executor` - define how to perform an action on a project

---

[edunext-cookiecutter]: https://github.com/eduNEXT/cookiecutter-openedx-plugin
[rg-cookiecutter]: https://gitlab.raccoongang.com/rg-developers/cookiecutter-openedx-plugin
[React]: https://reactjs.org
[Redux]: https://redux.js.org
[Nx]: https://nx.dev/
[nx-ws-generator]: https://nx.dev/generators/workspace-generators#workspace-generators