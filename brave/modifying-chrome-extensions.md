# Hacking Extensions

One of the great things about being a web developer is our ability to quickly learn from the work of others. In the mid 1990's, when I began learning HTML, _view source_ was my gateway to understanding and enlightenment.

The liberty to save a web page to my computer meant I could open it with Notepad, and begin making my own customizations. It was nice not having to bootstrap a project, in order to explore new and interesting ideas.

Extensions are a new frontier for many web developers. Sadly, they can't be studied as easily web pages. That being said, it's not too terribly difficult to explore and experiment with extensions as easily as we do with the web.

The goal of this short article is not to teach you how to create an extension. Rather, it's to grant you access to a working extension for the purposes of exploration and discovery. My goal is to get you through the door; the rest is up to you.

Once you have access to a working extension, and the ability to quickly make modifications, learning is often easier, and more enjoyable. If any part of this is unclear, feel free to reach out to [@jonathansampson](https://twitter.com/jonathansampson) on Twitter.

## Step 1: Find and install an extension

Installing extensions in Chrome is quite simple. Start by navigating to _about:extensions_ (or _chrome://extensions_). Get familiar with this page, we'll be poking around here more in the future.

At the bottom of about:extensions, you'll see a link with the text _Get More Extensions_. This takes us to the Chrome Web Store, where you can install extensions and more.

For our purposes, we'll be installing the [_Tabby Cat_ extension](https://chrome.google.com/webstore/detail/tabby-cat/mefhakmgclhhfbdadeojlkbllmecialg?hl=en-US). Note that when we're viewing the details of an extension, the unique ID of that extension will be displayed in the address bar; Tabby Cat's ID is _mefhakmgclhhfbdadeojlkbllmecialg_.

When you're ready, click the _Add to Chrome_ button. Once added, you should see an animated cat when you open a new tab. We'll modify this extension to announce each cat's name aloud.

## Step 2: Create a copy to modify

The first step in creating our own hackable copy of Tabby Cat is to determine where extensions are installed on our machine. On Windows, they will be located in your %localappdata% directory.

For me, the full path is:

> %localappdata%\Google\Chrome\User Data\Default\Extensions

Among the directories, you'll find a folder whose name matches the ID of the Tabby Cat extension. Open that folder to reveal another folder whose name matches the version of Tabby Cat we installed.

Copy the version folder, and paste it into the previous directory, where all of our other extensions reside. Name this new folder _Tabby Cat Plus_. At this point, we need to open the `manifest.json` file inside, and change the extension's name to "Tabby Cat Plus" as well.

Next, delete the `_metadata` folder (Once an extension is published, this folder contains unique hashes for security purposes). If you don't delete this folder, Chrome will refuse to let you complete the next step.

## Step 3: Load our custom copy

Now we'll need to load our extension into Chrome. Return to the about:extensions (or chrome://extensions) page, and enable _Developer Mode_ by clicking the checkbox near the top. Clicking this will reveal a _Load unpacked extension…_ button.

Click _Load unpacked extension…_, and select your _Tabby Cat Plus_ directory. This causes our copy of Tabby Cat (Tabby Cat Plus) to replace the original. At this point, you should see _Tabby Cat Plus_ listed instead of _Tabby Cat_.

## Step 4: Modify the original code

Now, it's time to make our modification. From the extension's root directory, open _public/scripts/tabby.js_. Inside, locate `var thisCat = {…}`. We'll add the following _after_ this assignment:

```js
speechSynthesis.speak(
  new SpeechSynthesisUtterance(
    `${thisCat.name.first} ${thisCat.name.last}`
  )
)
```

Save your changes, and return to Chrome. You should now find that opening a New Tab will not only show you an adorable kitten, but it will announce the kitten's name as well.

## Conclusion

That's it; you're up and running!

From here, explore the contents of the `manifest.json` file. Every extension you encounter will have one of these, and many of the properties within are **required** for the extension to work.

Be sure to bookmark, and explore, the [Web Extensions documentation](https://developer.mozilla.org/en-US/Add-ons/WebExtensions) on the Mozilla Developer Network. Note that some subtle differences will exist between various browsers, but you can more easily spot those now that you have access to an _already working_ extension.