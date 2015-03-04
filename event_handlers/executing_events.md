# Executing Events

Apart from executing events from the URL/FORM or Remote interfaces, you can also execute events internally, either **public** or **private** from within your event handlers or from interceptors, other handlers, layouts or views. You do this by using the <code>runEvent()</code> method which is inherited from our <code>FrameworkSuperType</code> class. Here is the method signature:

```js
/**
* Executes events with full life-cycle methods and returns the event results if any were returned.
* @event The event string to execute, if nothing is passed we will execute the application's default event.
* @prePostExempt If true, pre/post handlers will not be fired. Defaults to false
* @private Execute a private event if set, else defaults to public events
* @defaultEvent The flag that let's this service now if it is the default event running or not. USED BY THE FRAMEWORK ONLY
* @eventArguments A collection of arguments to passthrough to the calling event handler method
* @cache.hint Cached the output of the runnable execution, defaults to false. A unique key will be created according to event string + arguments.
* @cacheTimeout.hint The time in minutes to cache the results
* @cacheLastAccessTimeout.hint The time in minutes the results will be removed from cache if idle or requested
* @cacheSuffix.hint The suffix to add into the cache entry for this event rendering
* @cacheProvider.hint The provider to cache this event rendering in, defaults to 'template'
*/
function runEvent;
```