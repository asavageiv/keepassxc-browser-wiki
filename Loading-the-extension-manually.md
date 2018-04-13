### Loading the extension manually

If you have downloaded the sources from GitHub and you want to test or make a new feature you have to do some manual tweaking to get everything working.

This guide is needed for Chromium-based browsers only. With Firefox you can use `web-ext`.

### Modify the native messaging JSON script

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