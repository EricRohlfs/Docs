Diagnostics
===========

By `Steve Smith`_

ASP.NET 5 includes a number of new features that can assist with diagnosing problems.

.. contents:: Sections
	:local:
	:depth: 1

`Browse or download samples on GitHub <https://github.com/aspnet/Docs/tree/master/aspnet/fundamentals/diagnostics/sample>`_.

The runtime info page
---------------------

You can add a runtime info page by simply calling an extension method in the ``Configure`` method of ``Startup.cs``:

.. code-block:: c#

	if (env.IsDevelopment())
	{
	    app.UseRuntimeInfoPage(); // default path is /runtimeinfo
	}

Once this is added to your ASP.NET application, you can browse to the specified path (``/runtimeinfo``) to see information about the runtime that is being used and the packages that are included in the application, as shown below:

.. image:: diagnostics/_static/runtimeinfo-page.png

The path for this page can be optionally specified in the call to ``UseRuntimeInfoPage()``. It accepts a `RuntimeInfoPageOptions <https://github.com/aspnet/Diagnostics/blob/dev/src/Microsoft.AspNet.Diagnostics/RuntimeInfoPageOptions.cs>`_ instance as a parameter, which has a ``Path`` property. For example, to specify a path of ``/info`` you would call ``UseRuntimeInfoPage()`` as shown here:

.. code-block:: c#

	if (env.IsDevelopment())
	{
	    app.UseRuntimeInfoPage("/info");
	}

Diagnostic information should not be exposed in production. The code above prevents diagnostics outside the development environment.

.. note:: Remember that the ``Configure()`` method in ``Startup.cs`` is defining the pipeline that will be used by all requests to your application, which means the order is important. If for example you move the call to ``UseRuntimeInfoPage()`` after the call to ``app.Run()`` in the examples shown here, it will never be called because ``app.Run()`` will handle the request before it reaches the call to ``UseRuntimeInfoPage``.

The developer error page
------------------------

You can view the details of unhandled exceptions by specifying a developer error page. This topic is described in :doc:`error-handling`.


The welcome page
----------------

Another extension method you may find useful, especially when you're first spinning up a new ASP.NET 5 application, is the ``UseWelcomePage()`` method. Add it to ``Configure()`` like so:

.. code-block:: c#

    app.UseWelcomePage();

Once included, this will handle all requests (by default) with a cool hello world page that uses embedded images and fonts to display a rich view, as shown here:

.. image:: diagnostics/_static/welcome-page.png

You can optionally configure the welcome page to only respond to certain paths. The code shown below will configure the page to only be displayed for the ``/welcome`` path (other paths will be ignored, and will fall through to other handlers):

.. code-block:: c#

	app.UseWelcomePage("/welcome");

Glimpse
-------

Glimpse is a plug-in that provides a tremendous amount of insight into your ASP.NET application, directly from the browser. Glimpse can be added to your in app in just a few simple steps:

- Add a dependency on the "Glimpse" package in ``project.json``
- Call ``services.AddGlimpse`` in ``ConfigureServices``
- Call ``app.UseGlimpse`` in ``Configure``

Run your app on localhost, and you should see Glimpse information bar at the bottom of the browser window. `View a walkthrough of setting up Glimpse for ASP.NET Core RC1 <http://blog.getglimpse.com/2015/11/19/installing-glimpse-v2-beta1/>`_.

Logging
-------

ASP.NET includes a great deal of built-in logging that can assist with diagnosing many app issues. In many cases, just enabling logging is sufficent to provide the diagnostic information developers need to identify problems with their app. Logging is enabled and configured in your app's ``Startup`` class.

:doc:`Learn more about configuring logging in your ASP.NET app <logging>`.

.. note:: `Application Insights <https://azure.microsoft.com/en-us/documentation/articles/app-insights-asp-net-five/>`_ can provide production diagnostic information in a cloud-based, searchable format.
