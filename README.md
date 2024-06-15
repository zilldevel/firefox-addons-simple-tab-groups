<h2><a href="https://github.com/zilldevel/firefox-addons-zilldevel-tabgroups/releases/latest" target="_blank" rel="noopener noreferrer"><img width="48" src="https://raw.githubusercontent.com/zilldevel/firefox-addons-zilldevel-tabgroups/master/addon/src/icons/icon.svg" alt="zilldevel-tabgroups"></a> zilldevel-tabgroups</h2>

This project is a fork of [Simple Tab Groups](https://github.com/Drive4ik/simple-tab-groups) by Drive4ik. I (zilldevel) made it for testing out my own customizations independent of the original project. This project is not intended to replace STG or steal its glory. I plan to submit any worthwhile changes upstream for consideration but ultimately I plan to keep my changes present here for my own use, regardless of whether or not they are accepted upstream. Any who like my changes are welcome to use this version but I would encourage any users who are unfamiliar with STG to instead check out the original project first before deciding to use my forked version.

Some other notes:

- The name and logo changes are my attempts to better comply with the license. Not sure if that was actually required or not but, even if not, I would rather be respectful wherever possible by doing my best to comply with original author's wishes.

- I plan to do machine translations for any PRs I send off to upstream, but outside of that, I will probably slack and only do English. Translations are always welcome but I encourage you to submit them upstream wherever possible.

- This branch will contain any branding/logo/manifest changes as well as tested changes. As I had been new to the process, I originally had assumed (incorrectly) that I had to publish to AMO in order to get a signed build and be able to install my version in stable versions of Firefox and submitted a build to AMO. That build had been approved initially but then was rejected/removed by a different reviewer several days later on the grounds of this being a fork. Since apparently forks are a no-go for AMO and I have since realized that [signing](https://extensionworkshop.com/documentation/publish/signing-and-distribution-overview/) can be done without submitting the addon as a public AMO download, my plan for now is to just create a signed xpi and offer it under the Releases section on this site. If my fork ever diverges from upstream or upstream sees no activity for some time (e.g. see [issue 995](https://github.com/Drive4ik/simple-tab-groups/issues/995)) or someone requests it, then I might revisit this but as long as it's just me using it, I probably won't bother.

- For now, my versioning standard will be <upstream version>.<my version> e.g. if upstream version is 5.2.0 then my first build will be 5.2.0.1, second 5.2.0.2 and so on.

**Please support the original project / developer.**

Outside of this section and the next, I will be mostly leaving the original README text from upstream so in those sections "I" will probably be in the context of Drive4ik unless otherwise specified.



# Differences from original Drive4ik version

Aside from branding and logo changes made in an attempt to be compliant w STG's license terms, the differences are as follows:

Implemented:

- **Add ability to completely disable ALL notifications**: STG is not designed to work in private browsing mode. But rather than just silently doing nothing when you open a private browsing session, it squawks and bitches about it with a notification message. But it gets worse: not only do you see this notification when you first open the private browsing session, you will get it again EVERY time another addon opens a window (I have confirmed this with STG + DownThemAll in private browsing mode). I initially added an option to disable the notification about private browsing (see the [option-no-priv-browsing-notifications branch](https://github.com/zilldevel/firefox-addons-zilldevel-tabgroups/tree/option-no-priv-browsing-notifications)) but found that I would still occassionally get other notification messages while in private browsing. So I replaced the original option with one that allows you to disable ALL notifications for the entire addon (see the [option-to-disable-all-notifications branch](https://github.com/zilldevel/firefox-addons-zilldevel-tabgroups/tree/option-to-disable-all-notifications) / [upstream PR 1173](https://github.com/Drive4ik/simple-tab-groups/pull/1173)). Added in v5.2.0.1

Planned:

- STG conflicts with several addons. In my own testing of the original addon, I found that if you install one of those conflicting addons (e.g. TabStash) while in private browsing mode, not only do you get some nag screens from STG about this, but there appears to be a bug where it goes into an infinite loop opening tons of notification windows until you either kill the browser process (e.g. via remote ssh commands), the system kills the browser process, or the system runs out of memory and reboots. I plan to retest this and try to capture a video of this behavior using the original (unmodified) addon and file a ticket on the original project. If the issue isn't too complex, I may also consider creating a patch for it.
- Not committing to anything but there are a few existing STG issues that I wouldn't mind seeing resolved / features added for. Such as [1147](https://github.com/Drive4ik/simple-tab-groups/issues/1147), [768](https://github.com/Drive4ik/simple-tab-groups/issues/768)



## Translations

My fork is unlikely to do any translations outside of feature branches which might be submitted as PRs to the original project. In those cases, I will usually use online translation services.

For this branch, I am not likely to do so. I am open to PRs for language changes in any of my branches, provided they are related to my changes.

However, most translations should be submitted upstream whenever possible. Please, help Drive4ik [translate STG](https://drive4ik.github.io/simple-tab-groups/translate/index.html) into your language!

## Building

```bash
$ cd addon
$ npm install
$ npm run build
```

### `npm run build`

Build the extension into `addon/dist` folder for **development**.

### `npm run build-prod`

Build the extension into `addon/dist` folder for **production**.

### `npm run watch`

Watch for modifications then run `npm run build`.

### `npm run watch-prod`

Watch for modifications then run `npm run build-prod`.

### `npm run build-zip`

Build a zip file following this format `<name>-v<version>-(dev|prod).zip`, by reading `name` and `version` from `manifest.json` file.
Zip file is located in `dist-zip` folder.

On Fedora Linux, I use the following to build:

    $ cd addons
    $ npm install && npm run build && npm run build-zip

This will generate an unsigned addon as a zip file under `./addon/dist` (e.g. `<name>-v<version>-prod.zip`)

### creating a private signed xpi

Firefox stable build does not allow installing unsigned addons. Assuming that you have npm setup and are able to make clean builds, you have an AMO developer account, and have created a key from the [AMO Developer Hub](https://addons.mozilla.org/en-US/developers/addon/api/key/), then you can follow the steps for [getting started with web-ext](https://extensionworkshop.com/documentation/develop/getting-started-with-web-ext) to make sure `web-ext` is installed (e.g. `npm install --global web-ext` and make sure your `~/node_modules/.bin` is added to your `PATH` variable).

Then you should be able to create a signed xpi that can be installed manually by:

    $ AMO_JWT_ISSUER='<your JWT issuer value>'
    $ AMO_JWT_SECRET='<your JWT secret value>'
    $ cd addons
    $ npm install && npm run build
    $ cd dist
    $ web-ext lint
    $ web-ext build
    $ web-ext sign -v --channel unlisted --api-key "${AMO_JWT_ISSUER}" --api-secret "${AMO_JWT_SECRET}"
    $ cd web-ext-artifacts

For me the `web-ext sign ...` command took just shy of 5 minutes to run (over openvpn). Your mileage may vary.


## Description

zilldevel-tabgroups works across browser instances/windows too. If you select a group in another window, the selected window will jump to the foreground with the chosen group selected. You can even select the specific tab within that group in background browser windows. [GIF example](https://user-images.githubusercontent.com/7843031/33828871-806ccf6e-de76-11e7-9a0e-1ddfb97e878d.gif)

This allows for easy switching between active and pre-loaded tabs across multiple browser windows.

### This extension has these plugins:

 * **NEW** [Group notes](https://addons.mozilla.org/firefox/addon/stg-plugin-group-notes/)
 * [Create new group](https://addons.mozilla.org/firefox/addon/stg-plugin-create-new-group/)
 * [Create new tab](https://addons.mozilla.org/firefox/addon/stg-plugin-create-new-tab/)
 * [Create new tab in temporary container](https://addons.mozilla.org/firefox/addon/stg-plugin-create-temp-tab/)
 * [Delete current group](https://addons.mozilla.org/firefox/addon/stg-plugin-del-current-group/)
 * [Load custom group](https://addons.mozilla.org/firefox/addon/stg-plugin-load-custom-group/)
 * [Open Manage groups](https://addons.mozilla.org/firefox/addon/stg-plugin-manage-groups/)

Allow support message actions from Gesturify addon.
Allow import groups from addons "Panorama View" and "Sync Tab Groups".

### Work with Gesturefy addon
[How to configure the work with the plugin Gesturefy](https://user-images.githubusercontent.com/7843031/44263498-dffb1b00-a227-11e8-95c7-1b9474199ef0.png)

You have to copy and paste into Gesturefy addon

`Add-on ID` : `zilldevel-tabgroups@zilldevel`

`Parse message` -> `On`

Supported actions:
* `{"action": "add-new-group"}`
* `{"action": "rename-group"}`
* `{"action": "load-next-group"}`
* `{"action": "load-prev-group"}`
* `{"action": "load-next-unloaded-group"}`
* `{"action": "load-prev-unloaded-group"}`
* `{"action": "load-next-non-empty-group"}`
* `{"action": "load-prev-non-empty-group"}`
* `{"action": "load-history-next-group"}`
* `{"action": "load-history-prev-group"}`
* `{"action": "load-first-group"}`
* `{"action": "load-last-group"}`
* `{"action": "load-custom-group"}`
* `{"action": "delete-current-group"}`
* `{"action": "open-manage-groups"}`
* `{"action": "move-selected-tabs-to-custom-group"}`
* `{"action": "discard-group"}`
* `{"action": "discard-other-groups"}`
* `{"action": "reload-all-tabs-in-current-group"}`
* `{"action": "create-temp-tab"}`
* `{"action": "create-backup"}`


This extension may conflict with other programs similar in functionality.
Conflicted addons:
 * Tab Open/Close Control
 * OneTab
 * Tiled Tab Groups
 * Totally not Panorama (Tab Groups with tab hiding)
 * Panorama Tab Groups
 * Panorama View (etc.)

Open popup shortcut: `F8`. [You can change this hotkey](https://support.mozilla.org/kb/manage-extension-shortcuts-firefox)

Current list of functionality / development notes:

 * Design like old add-on "Tab Groups"
 * Added colored group icon
 * Added the ability to import the backup groups of the old plug-in "Tab Groups"
 * Added support of "Firefox Multi-Account Containers"
 * Now fully supports multiple windows
 * Saves last active tab after change group
 * Show currently used group in addon icon (see screenshot)
 * Specially NOT supported Private (Incognito) Mode
 * Added close tab by middle mouse click
 * Added simple switching between groups and tabs in search mode using the up, down, right and left keys
 * "Manage groups" functional is here! (so far only "Grid")
 * Added support Drag&Drop for tabs and groups in popup window
 * Added support sorting groups (context menu in popup window)
 * Added field for search/filter tabs in "Manage Groups"
 * Added support to Backup/Restore tabs, groups and settings to/from json file
 * Custom group icons, set group icon from tab icon (by context menu)
 * Added undo remove group by context menu browser button (see in screenshots)
 * Added support for catch tabs by containers (#76)
 * Added dark theme
 * Added support SideBar


Permissions used:
 * **tabs**: for tab handling
 * **tabHide**: for hide tabs
 * **contextualIdentities & cookies**: for work with Firefox Multi-Account Containers
 * **notifications**: for notification on move tab to group etc.
 * **menus**: for creating tabs context menus
 * **sessions**: for save session data (last used group, etc)
 * **downloads**: for create auto backups
 * **management**: for automatically detect the required addons
 * **storage**: for saving groups localy
 * **unlimitedStorage**: restore tabs after close window, there can be a lot of tabs
 * **<all_urls>(Access your data for all websites)**: for tab thumbnails and catch/move/reopen tabs in needed containers/groups
 * **webRequest** & **webRequestBlocking**: for catch/move/reopen tabs in needed containers/groups</li>
 * **(optional) bookmarks**: access for create bookmarks

## License and Credits

This project is licensed under the terms of the [Mozilla Public License 2.0](LICENSE).
