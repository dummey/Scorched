Milestones
==========

Changelog
---------
### v0.9
* Refactored `render` method:
    * All Scorched options are now keyword arguments, including `:locals` which was added as a proper render option.
    * Scorched options are no longer passed through to Tilt.
    * `:tilt` option added to allow options to be passed directly to Tilt, such as `:engine`
    * Unrecognised options are still passed through to Tilt for convenience.
* Added template caching using Tilt::Cache.
    * Added `:cache_templates` config option. Defaults to true except for development.

### v0.8
* Changed `controller` method signature to accept an optional URL pattern as the first argument.
* Implemented a pass mechanism to short-circuit out of the current match and invoke the next match.
* Added `:auto_pass` configuration option. When true, if none of the controller's mapping match the request, the controller will `pass` back to the outer controller without running any filters.
* Sub-controllers generated with the `controller` helper are automatically configured with `:auto_pass` set to `true`. This makes inline sub-controllers even more useful for grouping routes.

### v0.7
* Logging preparations made. Now just have to decide on a logging strategy, such as what to log, how verbose the messages should be, etc.
* Environment-specific defaults added. The environment variable `RACK_ENV`s used to determine the current environment.
    * Non-Development
        * `config[:static_dir] = false`   * Development
        * `config[:show_exceptions] = true`       * `config[:logger] = Logger.new(STDOUT)`       * Add developer-friendly 404 error page. This is implemented as an after filter, and won't have any effect if the response body is set.
* `absolute`ethod now returns forward slash if script name is empty.

### v0.6
* `view_config` options hash renamed to ` `render_defaults`ch better reflects its function.

### v0.5.2
* Minor modification to routing to make it behave as a documented regarding matching at the directly before or on a path.
* Response content-type now defaults to "text/html;charset=utf-8", rather than empty.

### v0.5.1
* Added URL helpers, #absolute and #url
* Render helper now loads files itself as Tilt still has issues with UTF-8 files.

### v0.5
* Implemented view rendering using Tilt.
* Added session method for convenience, and implemented helper for flash session data.
* Added cookie helper for conveniently setting, retrieving and deleting cookies.
* Static file serving actually works now
    * Custom middleware Scorched::Static serves as a thin layer on top of Rack::File.
* Added specs for each configuration option.
* Using Ruby 2.0 features where applicable. No excuse not to be able to deploy on 2.0 by the time Scorched is ready for production.
    * Keyword arguments instead of `*args`ombined with ` `Hash === args.last`  * Replaced instances of `__FILE__`ith ` `__dir__`Added expected Rack middleware, Rack::MethodOverride and Rack::Head.
    
### v0.4
* Make filters behave like middleware. Inheritable, but are only executed once.
* Improved implementation of Options and Collection classes

### v0.3 and earlier
* Basic request handling and routing
* String and Regex URL matching, with capture support
* Implemented route conditions
    * Added HTTP method condition which the route helpers depend on.
* Added route helpers
* Implemented support for sub-controllers
* Implement before and after filters with proper execution order.
* Configuration inheritance between controllers. This has been implemented as the Options class.
* Mechanism for including Rack middleware.
* Added more route conditions e.g. content-type, language, user-agent, etc.
* Provide means to `halt` request.
    * Added redirect helping for halting and redirecting request
* Mechanism for handling exceptions in routes and before/after filters.
* Added static resource serving. E.g. public folder.



To Do
-----
Some of these remaining features may be reconsidered and either left out, or put into some kind of contrib library.

* If one or more matches are found, but their conditions don't pass, a 403 should be returned instead of a 404.
* Make specs for Collection and Options classes more thorough, e.g. test all non-reading modifiers such as clear, delete, etc.
* Add more view helpers, maybe?
  * Add helper to easily read and build HTTP query strings. Takes care of "?" and "&" logic, escaping, etc. This is
    intended to make link building easier.
  * Form populator implemented with Nokogiri. This would have to be added to a contrib library.
* Add Verbose logging, including debug logging to show each routing hop and the current environment (variables, mode, etc)


Unlikely
--------
These features are unlikely to be implemented unless someone provides a good reason.

* Mutex locking option - I'm of the opinion that the web server should be configured for the concurrency model of the application, rather than the framework.
* Using Rack::Protection by default - The problem here is that a good portion of Rack::Protection involves sessions, and given that Scorched doesn't itself load any session middleware, these components of Rack::Protection would have to be excluded. I wouldn't want to invoke a false sense of security


More things will be added as they're thought of and considered.
