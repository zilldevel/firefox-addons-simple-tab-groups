<h2><a href="https://addons.mozilla.org/firefox/addon/simple-tab-groups/" target="_blank" rel="noopener noreferrer"><img width="48" src="https://rawgit.com/Drive4ik/simple-tab-groups/master/addon/src/icons/icon.svg" alt="Simple Tab Groups - zilldevel fork"></a> Simple Tab Groups - zilldevel fork</h2>

Simple Tab Groups is developed and maintained by Drive4ik. This fork adds changes from zilldevel (me) so that I can test and use my own customizations independent of the original project. If I think my changes would be useful to a larger audience and they pass validation, I will likely attempt to creaete a PR upstream so that Drive4ik has the option to merge if they like.

I am mostly only concerned with my own changes and don't plan to support everything in the original addon. For example, I probably am not going to bother with doing google translates of text/localization except in feature branches related to PRs (translation PRs are still welcome, I just won't be doing them myself).

All of my addons.mozilla.org (AMO) specific changes will be made in this branch which can be considered as the master branch for my AMO published version. Any issues specifically related to changes in my project should be filed against my project. Otherwise, the issue(s) should be retested against the original addon and filed there.

My versioning standard will be: v<original-addon-version>.<fork-sub-version> (e.g. "5.2.0.1" => based off STG v5.2 code with my own internal version of "0.1").

Please support the original project / developer.


[![Mozilla Add-on](https://img.shields.io/amo/v/simple-tab-groups.svg)](https://addons.mozilla.org/firefox/addon/simple-tab-groups/) [![](https://img.shields.io/amo/d/simple-tab-groups.svg)](https://addons.mozilla.org/firefox/addon/simple-tab-groups/statistics/?last=365) [![](https://img.shields.io/amo/users/simple-tab-groups.svg)](https://addons.mozilla.org/firefox/addon/simple-tab-groups/statistics/usage/?last=365) [![](https://img.shields.io/amo/rating/simple-tab-groups.svg)](https://addons.mozilla.org/firefox/addon/simple-tab-groups/reviews/)

[![https://addons.mozilla.org/firefox/addon/simple-tab-groups/](https://addons.cdn.mozilla.net/static/img/addons-buttons/AMO-button_2.png)](https://addons.mozilla.org/firefox/addon/simple-tab-groups/)


# Differences from original Drive4ik version

Currently this is just a test build and there are no changes.

Most of my planned changes revolve around making STG less annoying when you aren't using it (e.g. when you are in private browsing mode) such as the following:

- STG is not designed to work in private browsing mode. I have no intention of changing that. However, currently STG creates a lot of annoying notifications when you have it installed and decide to run a one-off private browsing sessionn. I do plan on addressing the way STG does notifications so that it does not get in the way while private browsing.
- STG conflicts with several addons. In my own testing of the original addon, I found that if you install one of those conflicting addons while in private browsing mode, not only do you get some nag screens from STG about this, but there appears to be a bug where it goes into an infinite loop opening tons of notification windows until you either kill the browser process (e.g. via remote ssh commands), the system kills the browser process, or the system runs out of memory and reboots. I plan to retest this and try to capture a video of this behavior using the original (unmodified) addon and file a ticket on the original project. If the issue isn't too complex, I may also consider creating a patch for it.


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


## Description

Simple Tab Groups works across browser instances/windows too. If you select a group in another window, the selected window will jump to the foreground with the chosen group selected. You can even select the specific tab within that group in background browser windows. [GIF example](https://user-images.githubusercontent.com/7843031/33828871-806ccf6e-de76-11e7-9a0e-1ddfb97e878d.gif)

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

`Add-on ID` : `simple-tab-groups@zilldevel`

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
