# What's New With 4.3.0

ColdBox 4.3.0 is a minor release that addresses several issues and introduces some enhancements. You can see below the release notes.

## Module Parent Settings
We have now introduced a standardized approach to defining/overriding module settings in your module and overrides via the parent application.  You can now define a `moduleSettings` structure in your `config/ColdBox.cfc` which will hold all the override settings for any modules you have installed in your system.

Module developers can then create defaults for those settings in a modules's `ModuleConfig.cfc` via the `settings` structure.

```js
// myModule/ModuleConfig.cfc
component {
  function configure() {
    settings = {
      someSetting = "default",
      anotherSetting = "default"
    };
  }
}

// config/ColdBox.cfc
component {
  function configure() {
    moduleSettings = {
      myModule = {
        someSetting = "overridden" 
      }
    };
  }
}

// end result
{
  someSetting = "overridden",
  anotherSetting = "default"
}
```

## Global `invalidHTTPMethodHandler`

You can now define a global invalid HTTP method handler in your `coldbox` configuration structure:

```
coldbox = {
  invalidHTTPMethodHandler = "main.invalidHTTP"
}
```

This will provide your application with error consistency when building RESTFul services.

## New `allowedMethods` annotation for handlers

Your event handler actions can now define their allowed HTTP methods of execution via the `allowedMethods` annotation:

```js
function index( event, rc, prc) allowedMethods="GET,POST"{
...
}
```

## New convention `modules_app`

With the introduction of CommandBox we can now have tracked modules and un-tracked modules in a ColdBox application.  The default convention for modules called `modules` is now the default location of tracked CommandBox modules.  The new convention `modules_app` is for your own custom un-tracked modules.

What is tracked? Modules that are tracked by CommandBox and usually not added to source control.  CommandBox controls their installation, updating, etc.

## Interceptors get `rc` and `prc` references

All interceptor methods now receive a reference to `rc` and `prc` for convenience:

```
function preProcess( event, rc, prc, interceptData, buffer )
```


## String Builders

Internal concatenation tools and interceptor response buffers have been migrated to Java String Builders for a more awesome performance updates.

## Binary HTTP Content

You can now receive and decode binary HTTP content when doing RESTFul services

## Models in Modules accept aliases now

Models in Modules can now have name aliases via the `alias` annotation or binder alias definitions.

## HTTP Method Spoofing

Although we have access to all these HTTP verbs, modern browsers still only support `GET` and `POST`.  With ColdBox and HTTP Method Spoofing, you can take advantage of all the HTTP verbs in your web forms.

By convention, ColdBox will look for an `_method` field in the form scope.  If one exists, the value of this field is used as the HTTP method instead of the method that was made.  For instance, the following block of code would execute with the `DELETE` action instead of the `POST` action:

```html
<cfoutput>
<form method="POST" action="#event.buildLink('posts/#prc.post.getId()#')#">
    <input type="hidden" name="_method" value="DELETE" />
    <button type="submit">Delete</button>
</form>
</cfoutput>
```

You can manually add these `_method` fields yourselves, or you can take advantage of ColdBox's HTML Helper.

```html
<cfoutput>
#html.startForm( action = "posts.#prc.post.getId()#", method="DELETE" )#
    #html.submitButton( name = "Delete", class = "btn btn-danger" )#
#html.endForm()#
</cfoutput>
```



----

## Release Notes

### Bugs

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-479'>COLDBOX-479</a>] - onInvalidHTTPMethod not firing with SES Interceptor

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-512'>COLDBOX-512</a>] - ColboxProxy.cfc has reference to method tracer - which no longer exists in that component.

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-515'>COLDBOX-515</a>] - Exception handling broken for customErrorTemplate when application relative path used

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-517'>COLDBOX-517</a>] - CF mapping&#39;s for modules don&#39;t get created for proxy requests

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-520'>COLDBOX-520</a>] - Only remove the `scriptname` from the `pathinfo` if it is the first element in the `pathinfo`

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-521'>COLDBOX-521</a>] - afterAspectsLoad was missing from core

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-527'>COLDBOX-527</a>] - bug report view is not thread safe

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-528'>COLDBOX-528</a>] - Coldbox Event Cache discards the Content-Type when caching non renderdata results

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-529'>COLDBOX-529</a>] - Object in RC or PRC causes async interceptors to fail

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-534'>COLDBOX-534</a>] - getHTMLBaseURL returns with a double slash at the end,

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-537'>COLDBOX-537</a>] - ColdBox Cache Flash not discovering session/cookie as app has not loaded first

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-539'>COLDBOX-539</a>] - Private event actions are no longer executable (regression)

### New Features

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-371'>COLDBOX-371</a>] - Convention to override a module&#39;s settings in an application or parent module

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-505'>COLDBOX-505</a>] - New coldbox directive: invalidHTTPMethodHandler that will fire globally if an action is called with an invalid HTTP Method

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-511'>COLDBOX-511</a>] - New action annotation &quot;allowedMethods&quot; so you can allow inline annoation for allowed HTTP Verbs thanks to Nic Tunney

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-522'>COLDBOX-522</a>] - Add rc and prc available directly to custom error templates

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-525'>COLDBOX-525</a>] - HTTP Method Spoofing for Forms

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-530'>COLDBOX-530</a>] - Add &#39;modules_app&#39; as a default external location instead of manually adding it.

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-540'>COLDBOX-540</a>] - Interception points get reference to rc and prc now.

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-541'>COLDBOX-541</a>] - invokerasync was not passing the buffer

### Improvements

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-502'>COLDBOX-502</a>] - RequestBuffers now leverage string builders.

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-513'>COLDBOX-513</a>] - Calling processState from within an interceptor clears buffer

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-531'>COLDBOX-531</a>] - execute() in BaseTestCase now uses the default event (defined in config/ColdBox.cfc) if / is passed in as the route

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-532'>COLDBOX-532</a>] - event.getHTTPContent() doesn&#39;t work on binary encoding

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-536'>COLDBOX-536</a>] - Rendered output has ALWAYS a precedent empty line, delete it!

* [<a href='https://ortussolutions.atlassian.net/browse/COLDBOX-538'>COLDBOX-538</a>] - Alises in module models aren&#39;t picked up



