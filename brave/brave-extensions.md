# Loading Chrome Extensions in Brave

## What is Brave

Brave appeared on the scene last year, and has quickly made a name for itself. Out of the box, Brave users enjoy fewer ads and trackers online. Brave also includes safety benefits like the prevention of fingerprinting and malware.

## What are its limitations

Like all software, Brave has its limitations. For example, Brave doesn't delivery playful and adorable kittens each time I open a new tab. For that type of functionality, users tyipcally turn to browser extensions. Fortunately, Brave and Chrome share a common ancestor, and therefore have the capacity to share many of the same extensions.

## What are we going to do in this walkthrough?

In this walkthrough, we're going to clone Brave to our local machine, install its dependencies, download a Chrome extension, and load it into Brave. It is my goal that by the end of this document, you will be equipped to explore the same process with many other extensions, and help our team achieve broader support.

## Cloning Brave and installing dependencies

Assuming you already have git, npm and nodejs installed, getting started with Brave is very straight-forward and quick. We'll start by cloning Brave's _browser-laptop_ repo onto our local machine.

Determine where you'd like to copy the Brave source code on your machine, and launch a command window from that directory. I will be working directly out of my `c:\` directory.

We'll continue by running the `clone` command from our target directory:

> git clone https://github.com/brave/browser-laptop.git

This step shouldn't take long. Once completed, we'll need install our dependencies.

> cd browser-laptop & npm install

This will take a little longer than the cloning process, but shouldn't last for more than a few minutes. When our dependencies have been installed, we can take a quick break from this walk-through and enjoy the fruits of our labor.

Open two terminals, and run `npm run watch` from the first. This launches the WebPack developer server. Once the server has been stood up, switch your focus to the second terminal, and run `npm start`. Within a couple of moments, Brave should appear on your screen.

Take a break, and pat yourself on the back. You've made it this far!

## Downloading Chrome Extensions

Now that we have the source of Brave on our machine, and we're able to run it, we can continue. If you haven't already, close all instances of Brave, and close both terminal windows. It's time to download an extension.

Typically, downloading a Chrome extension means first installing the extension in Chrome. This alone requires several steps. Once that is finished, we have to go digging through our file system to find the install directory, and copy a handful of files. That's too much work; fortunately, we have a shortcut.

Open a new terminal window from the `browser-laptop` directory, and run the following command. This performs a global install of the Chrome Extension Downloader (ced) module from npm. This module goes directly to Chrome's extension server, pulls down the packaged extension, and unpacks the archive.

> npm install -g chrome-ext-downloader

Once installed, it's time to go and get our extension. We'll want to make sure we install the extension in `app\extensions\` directory:

> cd app\extensions & ced mefhakmgclhhfbdadeojlkbllmecialg

The second command above calls _ced_, and asks it to download the Tabby Cat extension via the extension ID (mefhakmgclhhfbdadeojlkbllmecialg). Extension IDs can be found in the address bar when browsing the Chrome Web Store.

![Locating an extension ID](media/chrome-web-store-extension-id.png)

For the purpose of brevity, and keeping things simple, I'm renaming the new folder to `tabbycat`.

## Registering and loading the extension

At this point, we should have at least three folders in our `app\extensions` directory: brave, torrent, and tabbycat.

We now need to instruct our development build of Brave to register and load the Tabby Cat extension on startup. To do this, we need to open the `app\extensions.js` file, and locate the following lines:

```
// Manually install the braveExtension and torrentExtension
extensionInfo.setState(config.braveExtensionId, extensionStates.REGISTERED)
loadExtension(config.braveExtensionId, getExtensionsPath('brave'), generateBraveManifest(), 'component')
```

Beneath these lines we need to isnert the following:

```
// Enable Tabby Cat
extensionInfo.setState('tabbycat', extensionStates.REGISTERED)
loadExtension('tabbycat', getExtensionsPath('tabbycat'))
```

The first line is a simple comment for helping us find our way back if we ever get lost. The second line contains the `extensionInfo.setState` method, which we'll be using to register our extension. Lastly, the `loadExtension` method tells Brave where to look for Tabby Cat's `manifest.json` file.

## Testing and Troubleshooting

It's time to run Brave again. Open two command windows, and place your cursor in the first, and run `npm run watch`. Wait for the WebPack developer server to finishing loading. Once loaded, place your cursor in the second command window and run `npm start`.

Our first order of business is to confirm the extension has been loaded. We'll do this by navigating to the _about:extensions_ URL in Brave. We should see the extension listed among others.

![Brave's About Extensions Page](media/about-extensions.png)

_Disclaimer: At the time of this writing, the about:extensions page may not display anything at all. The reason for this can be quickly determined by pressing Ctrl+Shift+I, and inspecting the console output. Brave currently expects all `manifest.json` files to include a set of permissions. Tabby Cat does not contain a permissions array, which causes an error when the Array.prototype.join method is expected. [Pull Request #6581](https://github.com/brave/browser-laptop/pull/6581) will resolve this when it is merged into the master branch. A temporary work-around is to add `permissions: []` to the manifest._

## Filing bugs

Within a couple of seconds I also see that new tabs don't show adorable kittens. Taking a quick glance at the project's `manifest.json` file reveals the means by which the author intended to accomplish this: `chrome_url_overrides.newtab`. I now have two jobs: first is to file an issue on GitHub, and second is to find an alternative approach to delivering my much-needed kittens.

I can tell from the manifest.json file that my goal is to see the `public\index.html` file. I can use the `chrome-extension://` protocol in Brave to access these files. Using the ID found in `about:extensions`, I find that the kittens have been waiting for me all along at _[chrome-extension://fiiifhpa…bcmppmgc/public/index.html](chrome-extension://fiiifhpagigdpbnjaphepnmfbcmppmgc/public/index.html)_. Within a few seconds, I have them set as my homepage in _Settings_.

![Setting your homepage in Brave](media/brave-homepage.png)

The last step of this journey is to _use the extension_, and file issues for any broken behavior. This will help the Brave team address gaps in our API support, and more rapidly adopt the features needed for other extensions.

## Finding Assistance

This walk-through is intentionally superficial. You may encounter issues that were not thoroughly covered by this document. If you find yourself needing assistance, please consider joining http://community.brave.com. You may also [reach out to me on Twitter](https://twitter.com/bravesampson) and I'll help you find answers.
