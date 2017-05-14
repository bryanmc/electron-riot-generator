# App Setup File

This is an app created with the Electron cross-platform desktop.  These app generators are as much of a tool as a reference and educational resource.

## Package Details

Define your app's details below  to be used when running **npm-init **as values in **package.json**.

| Key | Value |
| :--- | :--- |
| package root directory | electron-riot |
| package name | electron-riot |
| package description | An electron app built with Riot JS and GBAG |
| package author | Bryan McConnahea |
| package keywords | electron, riot, gitbook-app-generator |
| package license | MIT |

> ### Initiating project folder and NPM package.json

```bash
mkdir %package root directory%
cd %package root directory%
npm init - y
```

> ### Updating package.json values with package details

```bash
json -I -f package.json -e 'this.name="%package name%"'
json -I -f package.json -e 'this.description="%package description%"'
json -I -f package.json -e 'this.author="%package author%"'
json -I -f package.json -e 'this.keywords="%package keywords%"'
json -I -f package.json -e 'this.license="%package license%"'
```





