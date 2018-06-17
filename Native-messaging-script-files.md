Here are the native messaging script files for manual adding or modification:

Firefox:
```
{
    "allowed_extensions": [
        "keepassxc-browser@keepassxc.org"
    ],
    "description": "KeePassXC integration with native messaging support",
    "name": "org.keepassxc.keepassxc_browser",
    "path": "<path to keepassxc-proxy>",
    "type": "stdio"
}
```

Chromium-based browsers:
```
{
    "allowed_origins": [
        "chrome-extension://oboonakemofpalcgghocfoadofidjkkk/"
    ],
    "description": "KeePassXC integration with native messaging support",
    "name": "org.keepassxc.keepassxc_browser",
    "path": "<path to keepassxc-proxy>",
    "type": "stdio"
}

```