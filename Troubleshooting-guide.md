Here's a general checklist for KeePassXC-Browser related connection issues:

### 1) After enabling Browser Integration and support for your browser
Depending on the operating system the `org.keepassxc.keepassxc_browser.json` native messaging script file shoud be installed to these locations:
#### Windows
`AppData\Local\keepassxc` or to the KeePassXC directory if using portable version.
Also there should be a registry entry found in the following location(s):
- Chrome: `HKEY_CURRENT_USER\\Software\\Google\\Chrome\\NativeMessagingHosts\\`
- Chromium: `HKEY_CURRENT_USER\\Software\\Chromium\\NativeMessagingHosts\\`
- Firefox: `HKEY_CURRENT_USER\\Software\\Mozilla\\NativeMessagingHosts\\`
- Vivaldi: `HKEY_CURRENT_USER\\Software\\Vivaldi\\NativeMessagingHosts\\`
- Tor Browser: `HKEY_CURRENT_USER\\Software\\Mozilla\\NativeMessagingHosts\\`
- Edge: `HKEY_CURRENT_USER\\Software\\Microsoft\\Edge\\NativeMessagingHosts\\`

#### Linux
- Chrome: `~/.config/google-chrome/NativeMessagingHosts`
- Chromium `~/.config/chromium/NativeMessagingHosts`
- Firefox: `~/.mozilla/native-messaging-hosts`
- Vivaldi: `~/.config/vivaldi/NativeMessagingHosts`
- Tor Browser: `~/.tor-browser/app/Browser/TorBrowser/Data/Browser/.mozilla/native-messaging-hosts`

Tor Browser installed via the official .tar.gz package will look for the following folders (when extracted to the home folder):
`/home/<user>/tor-browser_en-US/Browser/TorBrowser/Data/Browser/.mozilla/native-messaging-hosts/` and globally from `/usr/lib/mozilla/native-messaging-hosts/`.

Tor Browser installed via Torbrowser-launcher looks for the following (for en_US): `/home/<user>/.local/share/torbrowser/tbb/x86_64/tor-browser_en-US/Browser/TorBrowser/Data/Browser/.mozilla/native-messaging-hosts/`

For now, the .json file needed can be symlinked or copied to those directions, for example from under `.mozilla/native-messaging-hosts`.

#### macOS
- Brave: `~/Library/Application Support/BraveSoftware/Brave-Browser/NativeMessagingHosts`
- Chrome: `~/Library/Application Support/Google/Chrome/NativeMessagingHosts`
- Chromium: `~/Library/Application Support/Chromium/NativeMessagingHosts`
- Firefox: `~/Library/Application Support/Mozilla/NativeMessagingHosts`
- Vivaldi: `~/Library/Application Support/Vivaldi/NativeMessagingHosts`
- Tor Browser `~/Library/Application Support/TorBrowser-Data/Browser/Mozilla/NativeMessagingHosts`
- Edge `~/Library/Application Support/Microsoft Edge/NativeMessagingHosts`

If you use any other browser the configuration location can vary. In these cases the script file must be copied manually.

#### Other browsers
Some example Linux paths:
- Google Chrome Beta: `~/.config/google-chrome-beta/NativeMessagingHosts`
- Google Canary (Unstable): `~/.config/google-chrome-unstable/NativeMessagingHosts`
- Waterfox: `~/.waterfox/native-messaging-hosts`
- Iridium: `~/.config/iridium/NativeMessagingHosts`

### 2) Check the native messaging script file path and extension ID's
After finding the `org.keepassxc.keepassxc_browser.json` check the `path` variable inside it. It should point to the exact location to the KeePassXC binary. In Windows this variable can also exists without a path.

The extension ID under `allowed_origins` should be `chrome-extension://oboonakemofpalcgghocfoadofidjkkk/` for Chromium-based browsers and 
`keepassxc-browser@keepassxc.org` under `allowed_extensions` for Firefox. There can be another ID seen with Chromium. This is the older version of the extension and it will be removed in the future from the script file.

With Chromium-based browsers you can check that the extension ID is the same with what is shown at the extensions page.

### 3) Check if keepassxc-proxy is launched and running
Connection between KeePassXC and the browser extension is managed by `keepassxc-proxy` binary. Make sure this is up and running. If not try to start it manually. Under macOS the binary can be found inside the KeePassXC.app at `Contents/MacOS/`.

If the binary doesn't start on macOS, please check the binary linking with the following command:
`otool -L /Applications/KeePassXC.app/Contents/MacOS/keepassxc-proxy`
It should show the following for QtNetwork and QtCore frameworks:
`@executable_path/../Frameworks/QtNetwork.framework/Versions/5/QtNetwork`
`@executable_path/../Frameworks/QtCore.framework/Versions/5/QtCore`

It's possible to use KeePassXC without the proxy feature. This means the connection between the extension and KeePassXC is direct and KeePassXC must not be launched before the browser does it (at the startup or from the popup). This mean the extension cannot be connected to KeePassXC on-the-fly. Proxy can be disabled from the Advanced tab of Browser Integration settings. This is not the recommended setting.

### 4) Debug the extension itself
If everything else seems to be OK but you still have issues please try to debug the extension.

Chromium-based browsers:
- Go to the extension page, check `Developer mode`
- Click `backgroung page` link after the `Inspect views:` text

Firefox:
- Go to `about:debugging` and check `Enable add-on debugging`
- Find KeePassXC-Browser and click `Debug`

Console tab should now show any error messages the extension writes. If you are a developer and know these things you can use the Sources tab and debug the extension and find the exact point where it fails.

#### Debug the content script
For debugging autocomplete, password generator, or anything else that interacts with the web page itself, content scripts must be debugged.

Chromium-based browsers:
- Use the menu `View -> Developer -> Developer Tools` or with right mouse click on the page and select `Inspect`
- Go to `Sources` tab and use the double arrow on the top left of the Developer Tools to view `Content scripts`
- Open `top` tree and search for `KeePassXC-Browser`. Open `keepassxc-browser.js`

Firefox:
- Use the menu `Tools -> Web Developer -> Debugger`
- Go to `Sources` tab on the left and search for `KeePassXC-Browser`. Open `keepassxc-browser.js`
