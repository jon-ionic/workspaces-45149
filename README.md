# Versions used
- Ionic CLI: v6.20.8
- npm: 8.19.3
- node: v18.13.0

# Monorepo Example

Create a new npm project:
$ npm init

Add the following to your package.json:
```
  "workspaces": [
    "packages/example1"
  ],
```

Initialize an Ionic starter project in the packages directory:

`$ cd packages`

`$ ionic start example1 blank --type=react`

Note that a `capacitor.config.ts` file is not created automatically and Capacitor is not added as an integration in `ionic.config.json`.
Add Capacitor manually.

`$ cd example1`

`$ npx cap init example1 io.ionic.starter --web-dir build`

Modify your `ionic.config.json` to look like this:
```
{
  "name": "example1",
  "integrations": {
    "capacitor": {}
  },
  "type": "react"
}
```

Despite webDir being set in `capacitor.config.ts`, note that the output of `ionic deploy manifest --verbose` indicates that the Capacitor config file could not be found.

`$ npm run build`

`$ ionic deploy manifest --verbose`

```
  ionic:lib Terminal info: { ci: false, shell: '/bin/zsh', tty: true, windows: false } +0ms
  ionic:lib CLI global options: { _: [ 'deploy', 'manifest' ], help: null, h: null, verbose: true, quiet: null, interactive: true, color: true, confirm: null, json: null, project: null, '--': [] } +1ms
  ionic:lib:project Project type from config: @ionic/react (react) +0ms
  ionic:lib:project Project details: { context: 'app', type: 'react', errors: [], configPath: '/Users/jon/Projects/Tickets/workspaces-45149/packages/example1/ionic.config.json' } +0ms
  ionic Context: { binPath: '/Users/jon/.nvm/versions/node/v18.13.0/lib/node_modules/@ionic/cli/bin/ionic', libPath: '/Users/jon/.nvm/versions/node/v18.13.0/lib/node_modules/@ionic/cli', execPath: '/Users/jon/Projects/Tickets/workspaces-45149/packages/example1', version: '6.20.8' } +0ms
  ionic IONIC_TOKEN environment variable detected +21ms
  ionic:commands:deploy:manifest Getting config with Capacitor CLI: [ 'config', '--json' ] +0ms
  ionic:lib:telemetry Sending telemetry for command: 'ionic deploy manifest' [ '--verbose', '--interactive', '--color' ] +0ms
  ionic:commands:deploy:manifest Could not get config from Capacitor CLI (probably old version) +11ms
  ionic:commands:deploy:manifest Capacitor config file does not exist at '/Users/jon/Projects/Tickets/workspaces-45149/packages/example1/capacitor.config.json' +1ms
  ionic:commands:deploy:manifest Failed to load Capacitor config +0ms
  ionic:lib:integrations:capacitor Getting config with Capacitor CLI: [ 'config', '--json' ] +0ms
  ionic:lib:integrations:capacitor Capacitor config file does not exist at '/Users/jon/Projects/Tickets/workspaces-45149/packages/example1/capacitor.config.json' +3ms
  ionic:lib:integrations:capacitor Failed to load Capacitor config +1ms
[ERROR] The webDir property must be set in the Capacitor configuration file.
        
        See the Capacitor docs for more information:
        https://capacitor.ionicframework.com/docs/basics/configuring-your-app
  ionic:utils-process onBeforeExit handler: 'process.exit' received +0ms
  ionic:utils-process onBeforeExit handler: running 0 functions +0ms
  ionic:utils-process processExit: exiting (exit code: 1) +0ms
```

As a result, we cannot add live updates to this project.

`$ ionic deploy add --app-id="f33487bd" --channel-name="Production" --update-method="background"`

```
> npm i --save -E cordova-plugin-ionic

added 2 packages, and audited 1403 packages in 4s

233 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
> ionic deploy manifest
[ERROR] The webDir property must be set in the Capacitor configuration file.
        
        See the Capacitor docs for more information:
        https://capacitor.ionicframework.com/docs/basics/configuring-your-app
```

# Non-Monorepo Example

Outside of your npm project, create a new Ionic starter project.

`$ ionic start example1 blank --type=react`

Notice that `capacitor.config.ts` is created for you and `capacitor` is added as an integration in `ionic.config.json`.

We can build the project and execute `ionic deploy manifest --verbose` without issue.

`$ npm run build`

`$ ionic deploy manifest --verbose`
```
  ionic:lib Terminal info: { ci: false, shell: '/bin/zsh', tty: true, windows: false } +0ms
  ionic:lib CLI global options: { _: [ 'deploy', 'manifest' ], help: null, h: null, verbose: true, quiet: null, interactive: true, color: true, confirm: null, json: null, project: null, '--': [] } +1ms
  ionic:lib:project Project type from config: @ionic/react (react) +0ms
  ionic:lib:project Project details: { context: 'app', type: 'react', errors: [], configPath: '/Users/jon/Projects/Tickets/example1/ionic.config.json' } +0ms
  ionic Context: { binPath: '/Users/jon/.nvm/versions/node/v18.13.0/lib/node_modules/@ionic/cli/bin/ionic', libPath: '/Users/jon/.nvm/versions/node/v18.13.0/lib/node_modules/@ionic/cli', execPath: '/Users/jon/Projects/Tickets/example1', version: '6.20.8' } +0ms
  ionic IONIC_TOKEN environment variable detected +19ms
  ionic:commands:deploy:manifest Getting config with Capacitor CLI: [ 'config', '--json' ] +0ms
  ionic:lib:telemetry Sending telemetry for command: 'ionic deploy manifest' [ '--verbose', '--interactive', '--color' ] +0ms
  ionic:commands:deploy:manifest Loaded Capacitor config! +228ms
  ionic:lib:integrations:capacitor Getting config with Capacitor CLI: [ 'config', '--json' ] +0ms
  ionic:lib:integrations:capacitor Loaded Capacitor config! +216ms
[OK] Appflow Deploy manifest written to ./build/pro-manifest.json!
```