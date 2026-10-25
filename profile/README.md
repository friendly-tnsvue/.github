# Friendly TNSVue

Friendly TNSVue is a set of tools and experiments for using Vue with NativeScript. The repositories below are archived snapshots. This guide describes the code in those snapshots; it does not imply current compatibility or maintenance.

## Project map

| Line | Repository | Role |
| --- | --- | --- |
| Earlier | [friendly-tnsvue_old-migration-service](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service) | Earlier JavaScript migration engine with its own interactive workflow. |
| Prototype | [friendly-tnsvue_vue-plugin_01](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_01) | Incomplete experiment for registering layout and label components. |
| Prototype | [friendly-tnsvue_vue-plugin_02](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02) | README-only pointer to the preserved Vue plugin repository. |
| v0 | [ftns-cli](https://github.com/friendly-tnsvue/ftns-cli) | Standalone Gluegun CLI that dispatches migration commands to the v0 service. |
| v0 | [friendly-tnsvue-migration-service-v0](https://github.com/friendly-tnsvue/friendly-tnsvue-migration-service-v0) | TypeScript migration engine used by the v0 CLI. |
| v0 | [friendly-tnsvue-vue-plugin-v0](https://github.com/friendly-tnsvue/friendly-tnsvue-vue-plugin-v0) | Vue plugin that maps a small set of HTML-like components to NativeScript UI elements. |
| v1 | [friendly-tnsvue-v1](https://github.com/friendly-tnsvue/friendly-tnsvue-v1) | Lerna monorepo that brings together the CLI, migration service, and migration models. |

## How the pieces relate

The v0 CLI and migration service communicate in-process through a Gluegun command toolbox: the CLI registers the migrate command and delegates its run function to the service. The v0 service converts project files and installs NativeScript dependencies. The Vue plugin is a separate runtime dependency intended to provide the components used by converted templates.

The v1 monorepo brings the CLI, migration service, and migration models into one Lerna workspace. Its CLI asks which migration model to use, then passes that model's configuration to the service. The model supplies hooks, dependency lists, file transformation rules, and template behavior. The published package versions in the snapshots differ, so treat the v0 repositories and the v1 workspace as separate historical lines rather than interchangeable package versions.

## Earlier JavaScript migration service

Repository: [friendly-tnsvue_old-migration-service](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service)

This earlier implementation is organized around JavaScript robots for initialization, preparation, migration, and finishing, with Inquirer prompts and helper modules for package managers, project paths, package.json, and Vue template parsing. Its package has no bin executable. Loading src/index.js starts the interactive migration immediately; it does not export a reusable migration function.

The flow runs in the current working directory. It initializes Git, then creates an app-work branch, using a randomly suffixed branch name if that branch already exists. It asks which package manager to use after inspecting lockfiles, then asks for the source directory, entry point, and application ID. It may run the project's lint --fix script, adds NativeScript and plugin dependencies, updates JavaScript and Vue files, adjusts the entry point and router, writes the NativeScript application ID to package.json, generates template files, and removes residual files.

Because this snapshot runs Git commands and changes the project in place, inspect the source and ensure the target project's working tree is backed up before running it. The old README points to the Friendly TNSVue CLI, but the repository itself declares no CLI binary; verify package compatibility before combining it with another archived CLI. Its package lint script also uses ESLint with --fix and can modify files.


### Commit history

The repository began on April 23, 2019, as a small JavaScript migration project. Its May commits assembled the interactive workflow in stages: package-manager helpers and prompts came first, followed by preparation, file updates, CSS-to-layout conversion, template generation, and cleanup. The [initial commit](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service/commit/8671b30), the [file-update stage](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service/commit/3525112), and the [template stage](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service/commit/f97dce0) show that progression. The project reached its [v0.12.1 release](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service/commit/c968fb1) in May. Its latest recorded commit, from August 23, was a dependency update rather than a new migration capability ([commit](https://github.com/friendly-tnsvue/friendly-tnsvue_old-migration-service/commit/84406dc)).

## Plugin prototype 01

Repository: [friendly-tnsvue_vue-plugin_01](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_01)

This experiment separates layout and label components and includes a helper intended to register tags by component type. The layout draft maps a layout display value to NativeScript layout names; the label drafts wrap text in Label components.

The main install function calls a register function, but that function is empty in this snapshot. As a result, installing the package currently registers none of the draft components. The package also has no build or test scripts. Treat it as design material to inspect, not as a usable plugin. The 01 suffix is a prototype label, not a v1 release.


### Commit history

This prototype's history is concentrated on March 26, 2019. It began with a component-registration skeleton, then added a registration helper and separate drafts for labels and layouts ([registration](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_01/commit/eb21ea1), [labels](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_01/commit/01838f4), [layouts](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_01/commit/34059d7)). The final recorded changes combine those experiments, but the main registration function remained empty. The commit sequence therefore documents component ideas, not a completed registration pipeline.

## Plugin prototype 02

Repository: [friendly-tnsvue_vue-plugin_02](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02)

This repository contains only a README that points to the Vue plugin; it has no package manifest or implementation files. The preserved implementation is documented in [friendly-tnsvue-vue-plugin-v0](https://github.com/friendly-tnsvue/friendly-tnsvue-vue-plugin-v0). The 02 suffix identifies this pointer snapshot and does not indicate a v2 implementation.


### Commit history

The repository started on March 29, 2019, with plugin registration work, then added label and layout experiments ([initial commit](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02/commit/68c1409), [registration helper](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02/commit/bc58350), [layout draft](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02/commit/8e6934f)). On April 20, the [moved info commit](https://github.com/friendly-tnsvue/friendly-tnsvue_vue-plugin_02/commit/042f30b) removed the package manifest and implementation files, leaving a README pointer. That history explains why the repository is now README-only and why its earlier experiments should be treated as superseded by the preserved v0 plugin.

## v0: CLI

Repository: [ftns-cli](https://github.com/friendly-tnsvue/ftns-cli)

This is the standalone 0.2.1 CLI package. Its package manifest exposes both ftns and friendly-tnsvue-cli executables. Gluegun discovers the migrate command; that command is a thin adapter whose run function is the migration service. The short command alias is m.

The intended usage is to install the CLI and run it from the root of a Vue project:

~~~sh
npm install -g @friendly-tnsvue/cli
ftns migrate
~~~

The command runs locally and delegates through the Gluegun toolbox. It is not an HTTP client and does not call a hosted service. This CLI declares a dependency on migration-service 0.7.0-beta. Its build script compiles TypeScript, but also runs formatting and lint steps from the archived package configuration.


### Commit history

The CLI began as a Gluegun project on August 6, 2019. The [migrate command](https://github.com/friendly-tnsvue/ftns-cli/commit/235493d) and service integration followed three days later. Later changes added small command helpers and upgraded Gluegun, then replaced an environment-specific service import with the declared package dependency ([dependency integration](https://github.com/friendly-tnsvue/ftns-cli/commit/ce57417)). The recorded line ends at [v0.2.1](https://github.com/friendly-tnsvue/ftns-cli/commit/867ba6d) on December 10, 2019. The history is mostly the assembly of a thin command-line entry point around the separate migration service.

## v0: TypeScript migration service

Repository: [friendly-tnsvue-migration-service-v0](https://github.com/friendly-tnsvue/friendly-tnsvue-migration-service-v0)

This package is a Gluegun service, not a standalone executable. Its default export accepts a Gluegun toolbox and runs four stages: before-init, before-migrate, migrate, and finish. The CLI is the normal entry point because it supplies the toolbox and invokes the service.

During initialization, the service asks for the source directory, entry point, application ID, and package-manager mode. The default examples are src, main.js, and org.nativescript.application. The source directory must exist. Package-manager mode can use npm, Yarn, or update package.json without invoking an installer.

Before migration, the service runs lint --fix and adds the runtime and development dependencies defined by the selected model. During migration it applies configured file transformations, updates package.json and the entry point, and optionally offers to install nativescript-vue-navigator. The finish stage updates paths, generates the NativeScript template, presents next steps, and clears residual files.

The engine operates on the project directory represented by the toolbox and uses Gluegun prompts, filesystem and template helpers, plus package-manager commands. The configuration and model layers supply the operations executed by each stage. A direct integration must call the service with a compatible Gluegun toolbox; a plain Node object is not sufficient.

For the usual CLI flow, install the historical CLI and run ftns migrate from the Vue project root. In the service repository, yarn test runs the AVA tests and yarn build prepares and bundles the package. The archived build preparation includes formatting commands that can rewrite source files.


### Commit history

The service began on August 8, 2019, and grew from a basic package into a staged workflow. August commits added Git setup, prompts, package-manager handling, project preparation, file transformations, and finish-time templates. Later work added DOM helpers and router support, then changed the bundling setup and added tests. In November, the service gained a package.json-only mode and conversion of text nodes into NativeScript FormattedString and Span elements ([package-only mode](https://github.com/friendly-tnsvue/friendly-tnsvue-migration-service-v0/commit/127cfa7), [text conversion](https://github.com/friendly-tnsvue/friendly-tnsvue-migration-service-v0/commit/4fe1768)). A December commit marked v0.7.0-beta as unreleased. In January 2020, a [configuration refactor](https://github.com/friendly-tnsvue/friendly-tnsvue-migration-service-v0/commit/bd3e1a6) moved migration-specific behavior toward configurable models, an architectural direction carried into the v1 monorepo. Later history is largely dependency maintenance, with the latest recorded update in October 2020.

## v0: Vue plugin

Repository: [friendly-tnsvue-vue-plugin-v0](https://github.com/friendly-tnsvue/friendly-tnsvue-vue-plugin-v0)

This Vue 2 plugin registers two components through Vue.use: div and p. The div component converts its lStyle prop into layout properties and chooses a NativeScript layout from lStyle.display. Supported display values are absolute, dock, flex, grid, stack, and wrap; block and the default map to StackLayout. lStyle accepts either a CSS-like string or an object.

The p component renders its slot inside a NativeScript Label, wrapped by the div component. For example:

~~~js
import FriendlyTnsVue from '@friendly-tnsvue/vue-plugin'

Vue.use(FriendlyTnsVue)
~~~

~~~html
<div :lStyle="{ display: 'flex' }">
  <p>Hello from NativeScript-Vue</p>
</div>
~~~

The plugin registers components with the local Vue instance; it is not a migration engine and does not make a web HTML renderer available. Its package metadata targets the older Vue and NativeScript ecosystem, so check the application's versions before using it.


### Commit history

The plugin started on April 21, 2019, and added its div and p components the following day. Subsequent commits expanded lStyle parsing and layout mapping. The implementation briefly moved to TypeScript in July, then returned to JavaScript in October; that final iteration removed span, added a CSS-string parser, and fixed Vue component extension ([JavaScript return](https://github.com/friendly-tnsvue/friendly-tnsvue-vue-plugin-v0/commit/acbf748), [v0.7 release](https://github.com/friendly-tnsvue/friendly-tnsvue-vue-plugin-v0/commit/c55d7c2)). Feature work ended in October 2019. Later commits updated dependencies, with the last recorded update in February 2021.

## v1: monorepo

Repository: [friendly-tnsvue-v1](https://github.com/friendly-tnsvue/friendly-tnsvue-v1)

The workspace has three packages:

| Package | Responsibility |
| --- | --- |
| packages/cli | Gluegun executable branded as ftns. Provides help and version handling and registers the migrate command, with m as an alias. |
| packages/migration-service | TypeScript workflow engine. It receives a Gluegun toolbox and coordinates prompts, package-manager commands, file changes, and finishing steps. |
| packages/migration-models | Migration configurations. This snapshot exports a generic model that defines prompts, dependencies, file updates, and hooks consumed by the service. |

When invoked with ftns migrate, the CLI selects a model and injects its configuration into the migration service. The workflow runs preparation hooks, prepares the project and dependencies, transforms files, then runs the model's finishing hooks and help steps. The generic model includes Vue template and JavaScript transformations and a NativeScript template. The service uses Gluegun filesystem, prompt, template, and command helpers; it does not communicate with a remote conversion server.

To inspect or run the archived workspace in a suitable historical environment:

~~~sh
git clone https://github.com/friendly-tnsvue/friendly-tnsvue-v1.git
cd friendly-tnsvue-v1
yarn install
npx lerna bootstrap
cd packages/cli
yarn link
cd /path/to/vue-project
ftns migrate
~~~

The generic model asks for project paths and the application ID, and asks which package-manager mode to use. The migration can run lint with --fix, install dependencies, and rewrite project files. Review the target project's Git status and generated changes before accepting the result. Package scripts and dependencies are from an archived toolchain; check their side effects before running build or formatting commands.


### Commit history

The monorepo was initialized on January 23, 2020, as the migration service was being reshaped around configurable models. Early commits separated the models into their own package and added model selection to the CLI ([model package](https://github.com/friendly-tnsvue/friendly-tnsvue-v1/commit/a6b9c5c), [CLI selection](https://github.com/friendly-tnsvue/friendly-tnsvue-v1/commit/a3755d3)). Follow-up refactoring moved project-specific responsibilities into those configurations. The repository published alpha snapshots, reaching [1.1.1-alpha.0](https://github.com/friendly-tnsvue/friendly-tnsvue-v1/commit/0c9ac53) in January. The latest recorded commit is an October 2020 dependency update, so the history shows a short period of architectural development followed by dependency maintenance.
