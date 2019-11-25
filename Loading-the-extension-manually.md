### Loading the extension manually

If you have downloaded the sources from GitHub and you want to test or make a new feature you have to do some manual tweaking to get everything working.

With Firefox you can also use `web-ext`. See https://extensionworkshop.com/documentation/develop/getting-started-with-web-ext/ for further information.

### Modify the native messaging JSON script for Chromium

Load the extension from `chrome://extensions/` and enable `Developer mode`.
Check the KeePassXC-Browser ID:
![](https://i.imgur.com/BQYecoz.png)

Next, open the `org.keepassxc.keepassxc_browser.json` ([Troubleshooting guide](https://github.com/keepassxreboot/keepassxc-browser/wiki/Troubleshooting-guide) shows the locations) and add the extension debug ID to the json:
```
    "allowed_origins": [
        "chrome-extension://kdggfocfphncghpalgjiecgcnpgglmef/",
        "chrome-extension://oboonakemofpalcgghocfoadofidjkkk/"
    ],
```

When you reload the extension it should now connect to KeePassXC.

### Load the temporary extension to the browser

With Firefox, open `about:debugging#/runtime/this-firefox` and click _Load Temporary Add-on_. Browse to the unzipped folder and select `manifest.json` file.

With Chromium, open `chrome://extensions`, enable _Developer Mode_ from the top-right and click _Load unpacked button_. Select the unzipped folder.

## Build extension into .zip file and load

This step is only needed for deployment. It loads all the translations from Transifex and adjusts `manifest.json` separately for Firefox and Chromium-based browsers. For example, it allows to use SVG icons with Firefox.

### Prerequisites

- Run `npm install`.
- Install the [Transifex CLI tool](https://github.com/transifex/transifex-client#installation).
- Log into Transifex (e.g., with your GitHub account), click "Join team" on [this page](https://www.transifex.com/keepassxc/keepassxc-browser/) and, after a page reload, make sure "KeePassXC" is the active organization in the drop-down box on the top of the page.
- Go to your Transifex user settings, generate an API token, give it a name associating it with KeePassXC and [save it in your config file](https://docs.transifex.com/client/client-configuration).

### Building

Run `npm run build`.

### Installing 

This method installs the development version as a permanent extension. Use it only if you really need to, for example testing features with browser restarts.

#### Firefox

- Visit `about:config` and set `xpinstall.signatures.required` to `false`. This requires ESR, Developer Edition or Nightly. (Remember to undo this.)
- Visit `about:addons`.
- Go to the custom add-on settings and export a backup.
- Uninstall the add-on.
- Click the gear button, "Install Add-on From File..." and choose the built .zip file for Firefox.
- Restart the browser and connect your database via the settings.

#### Chromium-based browsers

Use the normal temporary extension loading feature described earlier.