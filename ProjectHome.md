# Debug Console JS #
Turns javascript `console` on and off, sets console logging level threshold, and automatically skips `console` methods that are not available in the current browser.

# About #
This simple library was made to highlight javascript `console` role as a debugging tool and pursuits the following goals:

  * No output from `console`, unless debug mode is explicitly turned on
  * `console` calls don't cause js exceptions if they are not supported, even if debug mode is on
  * `console` output could be easily turned on and fine tuned, i.e. allowing only `console.warn` logs or higher.
  * Very small footprint, doesn't require any existing code modifications to use and won't break any code when uninstalled

# Usage #
When `console.js` is included on a page:
```
<script src="console.js"></script>
```

all `console` calls will be automatically ignored, so no output from `console` in browsers that support it, and no exceptions in browsers that don't.

Console output is controlled through global var `DEBUG`, which could take the following values:

  * `<true|"on">` - console calls are enabled, calls not supported by a browser are silently ignored
  * `<false|"off">` - all console calls are silently ignored
  * `<"log"|"debug"|"info"|"warn"|"error">` - all `console` calls are disabled except logging calls with a logging level that is equal or greater than the provided threshold

Often it is needed to temporary enable console for a quick debug, you can do so without even modifying the `DEBUG` var value by appending `#debug` URL hash or `debug` URL parameter to the current page URL, for example:

```
http://example.com#debug (would require manual page refresh as adding hash doesn't reload the page)
http://example.com?debug
```

URL parameter is equal to setting `DEBUG=true` and overrides any existing `DEBUG` values.

# Examples #
DEBUG mode is off (default), all `console` calls are ignored:
```
<script src="console.js"></script>
<script>
	console.time("timer");		//ignored
	console.dir("DIR");		//ignored
	console.log("LOG");		//ignored
	console.warn("WARN");		//ignored
	console.error("ERROR");		//ignored
	console.timeEnd("timer");	//ignored
</script>
```

DEBUG mode is on, all supported `console` calls are allowed:
```
<script>
	DEBUG = true;
</script>
<script src="console.js"></script>
<script>
	console.time("timer");		//allowed
	console.dir("DIR");		//allowed
	console.log("LOG");		//allowed
	console.warn("WARN");		//allowed
	console.error("ERROR");		//allowed
	console.timeEnd("timer");	//allowed
</script>
```

DEBUG mode is on with the threshold, all `console` calls are disabled except logging calls with the logging level that is equal or greater than `warn` level:
```
<script>
	DEBUG = "warn";
</script>
<script src="console.js"></script>
<script>
	console.time("timer");		//ignored
	console.dir("DIR");		//ignored
	console.log("LOG");		//ignored
	console.warn("WARN");		//allowed
	console.error("ERROR");		//allowed
	console.timeEnd("timer");	//ignored
</script>
```