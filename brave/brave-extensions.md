# Loading Chrome Extensions in Brave

## What is Brave?

Brave&mdash;the new ad-clobbering Browser from Brendan Eich (creator of JavaScript, co-founder of Mozilla)&mdash;burst onto the scene last year. Out of the box it blocks ads, trackers, fingerprinting attempts and more.

For all that Brave does, it won't place random animated kittens on New Tabs&mdash;you'll need an extension for that. Fortunately, Brave was born out of the Electron project (itself born out of the Chromium project), which means Brave and Chrome are not-too-distant cousins, and can share quite a bit of code.

In this walk-through, we're going to clone the Brave browser to our local machine, and explore the process of registering and loading a Chrome extension in Brave.

Because Brave is still a very new browser, support for Chrome extensions is experimental, but growing. By helping the Brave team explore your favorite extension, support can be achieved more rapidly.

## Getting Brave's Source Code

One of the great things about Brave is the fact that it's light-weight. Like most websites, it basically consists of HTML, CSS, and JavaScript files. Assuming you have node and npm already installed, you can be up and running with Brave in under 8 minutes. And I mean that. I timed it. Twice. For Science.

Start by cloning the Brave project from GitHub:

> git clone https://github.com/brave/browser-laptop.git

This will create a `browser-laptop` directory. Enter that directory, and install all of the browser's dependencies:

> cd browser-laptop

> npm install

This step takes me about 7.5 minutes, consistently.

## Downloading an extension without Chrome

Downloading Chrome extensions usually means having to go into Chrome, navigate to the internal extensions resource page, click a link, find the extension, click a button, followed by half a dozen other steps. That's too much work.

Extensions can be downloaded directly from clients2.google.com. Adjusting one parameter on the URL determines which extension you'll be downloading. As an example, lets go ahead and download the Tabby Cat extension from my previous article.

Tabby Cat has an ID of mefhakmgclhhfbdadeojlkbllmecialg. The full download URL for this extension will be https://clients2.google.com/service/update2/crx?response=redirect&prodversion=55&x=id%3D<mark>mefhakmgclhhfbdadeojlkbllmecialg</mark>%26uc.

## Converting the extension from a CRX to a ZIP

Chrome extensions come packaged as [CRX files](https://developer.chrome.com/extensions/crx). They're effectively ZIP archives, but with a little extra information before the content. For example, the first 4 bytes of a CRX are "Cr24". The next 4 bytes tell us the CRX format number (usually `02 00 00 00`).

In order to unpack the CRX, we'll need to strip away the meta information. Fortunately, there's a great [tool](https://johankj.github.io/convert-crx-to-zip/) you can use to do this conversion. It run in the browser, using the File Reader API. Simply drag and drop the CRX onto the target, and download the resulting ZIP.

You may get a notification complaining that the Zip is invalid. This has happened for me with *some* extensions when using the aforementioned converter. Don't be alarmed; it's relatively easy to work around. And on the plus side, I get to show you another neat tool :)

## Extracting our extension

At this point, we will need to extract the contents of the ZIP into Brave's `app\extensions` directory. Initially, this directory should have a `brave` folder, as well as a `torrent` folder. Lets extract our ZIP into a `tabby-cat` directory.

## Registering and loading via app\extensions.js

With our extension unpacked, and in place, we now need it to be registered, and loaded. Open `app\extensions.js` and scroll down a few hundred lines until you find the following:

```
// Manually install the braveExtension and torrentExtension
extensionInfo.setState(config.braveExtensionId, extensionStates.REGISTERED)
loadExtension(config.braveExtensionId, getExtensionsPath('brave'), generateBraveManifest(), 'component')
```

Much of Brave's logic is actually bundled as an extension, and these lines bring that extension to life. We're going to copy these, make a couple minor modifications, and bring our Tabby Cat extension to life as well.

Beneath the lines seen above, lets add the following:

```
// Enable Tabby Cat
extensionInfo.setState('tabbycat', extensionStates.REGISTERED)
loadExtension('tabbycat', getExtensionsPath('tabbycat'))
```

The above three lines are fairly straight-forward: the first is a comment, the second sets the state of the _tabbycat_ extension as being _REGISTERED_. Lastly, the third line loads the extension. The last instance of `'tabbycat'` is our directory name.

The `getExtensionsPath` method is responsible for locating our `manifest.json` file. Since this file is directly inside the tabbycat folder, we need only to provide the folder name.

## Necessary modifications

This part of the tutorial has an expiration date. Presently, Brave doesn't support all of the extension APIs and features you'll find in Chrome (though we are actively closing that gap). For now, however, some extensions may require a few minor changes before they'll work, and Tabby Cat is no exception.

Befor we run Brave, and test Tabby Cat, we should make one small change to the `manifest.json` file. At the top, add `"permissions": []` to the object.

```
{
    "update_url": "https://clients2.google.com/service/update2/crx",
    "manifest_version": 2,
```

We should now have this:

```
{
    "permissions": [],
    "update_url": "https://clients2.google.com/service/update2/crx",
    "manifest_version": 2,
```

The absensce of this property will currently cause problems for the `about:extensions` page in Brave. Don't worry, [we've filed an issue](https://github.com/brave/browser-laptop/issues/6533) :)

## Loading Brave

Now that we've pulled down our extension, unpacked it, registered it, loaded it, and made a few minor modifications, we are ready to run the development build of Brave.

Start by opening two command windows from the `browser-laptop` directory. In your first window, run `npm install` (if you haven't already) to pull down all dependencies. This will take a few minutes.

Once our dependencies are installed, run `npm run watch` from the same window and wait for the process to finish. Finally, turn your focus to the second window, and run `npm start`. Within a few seconds, Brave will make its grand appearance on your screen.

### Identify, File, Fix!

## File Bugs and Discuss Efforts

    - Watch terminals, Brave console, and web content console
    - Explore extension pages using the `chrome://` protocol
    - File bugs online at github.com/brave/browser-laptop/issues
    - Discuss this process and more at community.brave.com