# Electron Screen Recorder App

Simple Electron Screen Recorder App with [Bulma](https://bulma.io/) following tutoral from Fireship Youtube Video [[Link](https://youtu.be/3yqDxhR2XxE)]

As a two years old video, the original code cannot working for the latest election framwork due to various Electron Breaking Changes introduced in 14 and 17. (v20.1.4 for this repo)

## How to Run

`npm start` will start the app | `rs` will refresh the app

## Fixed Problems

### Afer V14.0: `remote` module removed

we have to introdcued the remote module manually

changes made in `index.js` file

```javascript
// initialized electron/remote module
const remote = require('@electron/remote/main')
remote.initialize()

// Also webPreferences for BrowserWindow need to have following properties
webPreferences: {
    nodeIntegration: true,
    contextIsolation: false,
    enableRemoteModule: true,
    preload: path.join(__dirname, 'preload.js'),
},
```

changes made in `render.js` file

```javascript
// new Menu dialog from electron/remote not remote
const { Menu,dialog } = require('@electron/remote');
```

### Afer V18.0: `desktopCapturer.getSources` removed in render

Electron remove `desktopCapturer.getSources` API from rendering process. I use the solution proved by the [document](https://www.electronjs.org/docs/latest/breaking-changes).

changes made in `index.js` file

```javascript
const { app, BrowserWindow, ipcMain, desktopCapturer } = require('electron');

ipcMain.handle(
  'DESKTOP_CAPTURER_GET_SOURCES',
  (event, opts) => desktopCapturer.getSources(opts)
)
```

changes made in `render.js` file

```javascript
const { ipcRenderer } = require('electron');

const desktopCapturer = {
  getSources: (opts) => ipcRenderer.invoke('DESKTOP_CAPTURER_GET_SOURCES', opts)
}
```


## How to Package

```bash
npm run make
```
