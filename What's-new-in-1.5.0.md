1.5.0 is going to be a major release for the browser extension. The user experience has been changed slightly to perform logins more easily, just using a new username field icon that allows user to fill credentials with a single click. There's also some updated under the hood that improves input field detection for example Google Accounts and Protonmail login pages.

There's a more detailed explanation of the new features below.

### 1. Changes to username fields
Every username field (or a password field if it's a single one) has a username icon. Database can be unlocked by clicking the icon. If a database is already open, single credentials are automatically filled when clicked. If multiple credentials are available, autocomplete menu is shown instead. Clicking the input field directly shows the autocomplete menu, just like in previous versions.

Only one of these icons are shown even if there's multiple possible input fields visible.

![Screen Shot 2019-08-22 at 15 19 07](https://user-images.githubusercontent.com/24570482/63513986-3ca2ad00-c4f0-11e9-9191-64b4f7e0dc4c.png)

See it in [action](https://www.dropbox.com/s/xq2t3hw0nkze6qr/username_field.mp4?dl=0).

### 2. Changes to password generator and its icon(s)
Password generator icons are now invisible by default, and the Settings page's `Activate password generator` settings has been changed to `Activate password generator icons`. Also the context menu item for showing the icons has been removed. Password generator can still be shown via the context menu or keyboard shortcut even if the icons are invisible.

As using password generator feature is a less common task, it makes sense to hide it from the user and give more focus to the username field and credential filling instead.

### 3. Changes to notifications
The extension doesn't use much notifications, but when needed, using the official Notification API has a limited functionality on some browsers (e.g. showing button), plus notifications can be shown over browser toolbar icons which affects usability. 

All notifications are shown in the DOM instead, and these will disappear automatically after five seconds, or can be clicked away.

![Screen Shot 2019-09-17 at 19 13 13](https://user-images.githubusercontent.com/24570482/65060494-a348a880-d980-11e9-9db8-da4d0f464f77.png)

### 4. Changes to credential saving
In the previous versions, credential saving has been unreliable. Using the popup for saving credentials is a complex way to do it, and requires lots of extra code just to maintain some kind of usability for it. This PR modifies the process in a way that credentials are saved using a banner that is directly shown in the DOM.

All related features from the popup icon is removed with Settings page options for redirects etc, which are no longer needed at all. Even if the web page has some redirects, the credentials are saved temporarily to the background script where it can be retrieved when showing the credential banner.

![Screen Shot 2019-09-17 at 19 13 34](https://user-images.githubusercontent.com/24570482/65060504-a93e8980-d980-11e9-860e-5cf91ef1b763.png)

The banner feature can be seen [here](https://www.dropbox.com/s/olkqxd3exq138o9/banner.mp4?dl=0) (video).

### 5. Changes to Site Preferences
When the extension detects a page with a single username field, the popup will show an additional message that allows user to add the current site to Site Preferences with Username-only option enabled. Fields are also redetected automatically, so the page is ready for use right after the addition.

![Screen Shot 2019-09-03 at 14 08 52](https://user-images.githubusercontent.com/24570482/64169702-07368180-ce57-11e9-8452-d4ff289e3a89.png)

A new file `sites.js` is introduced in the content scripts. It holds a list of certain sites that needs special URL handling, and it also includes a function that allows common username-only sites to be added to Site Preferences by default (however, this feature is not used yet, as the list must be updated).

### 6. Changes to the password generator
The password generator is simplified and much more compact.

![Screen Shot 2019-09-17 at 19 14 24](https://user-images.githubusercontent.com/24570482/65060466-92983280-d980-11e9-9cd7-ceed48b97998.png)

### 7. Support for ignoring Auto-Submit

This version also supports ignoring Auto-Submit for certain entries. This feature will be released with KeePassXC 2.5.0. It contains a Browser Integration page for entries.