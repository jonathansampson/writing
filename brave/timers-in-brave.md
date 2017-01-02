# Timers in Brave

Brave contains numerous timers, configured to perform regular actions at predetermined intervals. This document seeks to enumerate those timers, their intervals, and their purpose.

It's relatively easy to locate timers in the Brave source; two methods can be taken to seek them out. The first method is to simply lookup calls to `setInterval` and `setTimeout`.

In Visual Studio Code, you can do a project-wide search by pressing <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F</kbd>. From the _Search_ panel, type "setInterval", and press <kbd>Enter</kbd>. Here are a few of the results as of the time of this writing:

### app\extensions\brave\content\scripts\passwordManager.js
Frequency: _Every 3 seconds, when the Password Manager is in use_

The call to `setInterval` here is fairly self-explanatory. When a stored password is retrieved, Brave will attempt to auto-fill the related form on the document.

### app\index.js
Frequency: _Every 5 minutes_

In this file, we'll see that `setInterval` is called every 5 minutes. Each call invokes the `initiateSessionStateSave` method. If the name of this method doesn't give it away, this timer is configured to record the current session storage for all Brave windows.

### app\ledger.js
### app\updater.js
Frequency: _Every 60 minutes_

This file is central to some very important functionality in Brave&mdash;core updates. Every hour (defined in _js\constants\appConfig.js_), the local `checkForUpdate` method is called. If a new build of Brave is available, the user will be notified.

### js\components\newTabComponents\clock.js
Frequency: _Every second, on New Tab Page_

A recent update to Brave brought users the _New Tab Page_. Among many helpful components on this page, the user is shown a small clock element. The _clock.js_ file contains a call to `setInterval` that keeps this clock element updated every second.

### js\webtorrent\entry.js

Frequency: _Every second, on the Torrent UI Page_

