# Fixing the Web with Auto-Responders


### What's the problem?

One of the things that makes Brave great is its ability to excise bad code before it ever touches your machine. Like a skilled surgeon, Brave carefully cuts away harmful data, with the intent of keeping the good data intact.

At times, a site's functionality may rely upon certain resources that happen to trigger Brave's suspicions. As a result, when these resources are blocked, the site may cease to function properly&mdash;even if the resource isn't crucial to the page's core purpose.

In most cases, you can repair these pages by disabling Brave's shields in part, or completely. This isn't a good solution for most though, as the idea of the web is that it just works out of the box. So Brave happens to have the ability to store and use individual site-hacks. We'll circle back to that here in a bit though.

The difficult thing about trying to fix the web is that we're dealing with _other people's code_, on _their servers_&mdash;we don't have the liberty of making local changes, refreshing, and seeing if we're able to resolve issues. We aren't entirely helpless though, thanks to Eric Lawrence.

### Okay, so what's the solution?

Fiddler is a web-proxy debugger that allows you to do many things, including the ability to create an _Auto-Responder_ for individual requests. Think of it as a way to edit local cached files. When the site is requested, the modified cache is loaded, and a trip to the server is avoided. The Brave team uses this tool (and others) to fix the web.

### What about the process?

- Open Fiddler
- Set a host filter (to keep the noise down)
- Clear all cache in your browser to ensure a full request
- Decode target sessions, and drop them into an auto-responder
- Edit the files in Fiddler, or save them to disk for convenience
- Issue request in Brave again; note changes in response
- Iterate until a fix has been identified

### I have a work-around, now what?

- Open an issue for the domain
- Identify the work-around for the issue
- Create the site-hack and issue a pull-request
- Save the Web