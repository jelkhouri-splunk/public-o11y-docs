.. _configure-ios-instrumentation:

*****************************************************************
Configure the Splunk iOS RUM instrumentation
*****************************************************************

.. meta::
   :description: Configure the Splunk Observability Cloud real user monitoring / RUM instrumentation for your iOS applications.

You can configure the iOS RUM agent from the Splunk OpenTelemetry Instrumentation for iOS to add custom attributes, adapt the instrumentation to your environment and application, customize sampling, and more.

To configure the iOS RUM agent, pass the settings as arguments when initializating the ``SplunkRum`` module. The following example shows how to configure the RUM token, beacon URL, and environment name:

.. tabs::

   .. code-tab:: swift
      :emphasize-lines: 3,4,5

      import SplunkOtel
      //..
      SplunkRum.initialize(beaconUrl: "https://rum-ingest.<realm>.signalfx.com/v1/rum",
            rumAuth: "<rum-token>",
            options: SplunkRumOptions(environment:"<environment-name>"))

   .. code-tab:: objective-c
      :emphasize-lines: 4,5,6

      @import SplunkOtel;

      //Create an options object to store the settings
      SplunkRumOptions *options = [[SplunkRumOptions alloc] init];
      options.environment = @"<environment-name>";
      [SplunkRum initializeWithBeaconUrl:@"https://rum-ingest.<realm>.signalfx.com/v1/rum" rumAuth: @"<rum-token>" options: options];

.. _ios-rum-settings:

General settings
======================================================

Use the following settings to configure the iOS RUM agent:

.. list-table:: 
   :header-rows: 1
   :widths: 20 80

   * - Option
     - Description
   * - :code:`beaconUrl`
     - Ingest URL to which the agent sends collected telemetry. The URL must contain your realm in Splunk Observability Cloud. For example, ``https://rum-ingest.us0.signalfx.com/v1/rum`` is the ingest URL for the ``us0`` realm.
   * - :code:`rumAuth`
     - RUM token that authorizes the agent to send telemetry data to Splunk Observability Cloud. To generate a RUM access token, see :ref:`rum-access-token`.
   * - :code:`globalAttributes`
     - Sets additional attributes added to all spans. Attributes are defined as an array of comma-separated key-value pairs. For example: ``["key1":"value1","key2":3]``. See :ref:`ios-rum-globalattributes`.
   * - :code:`environment`
     - Environment for all the spans produced by the application. For example, ``dev``, ``test``, or ``prod``.
   * - :code:`ignoreURLs`
     - Regular expression pattern that matches URLs you want to ignore when reporting HTTP activity.
   * - :code:`spanFilter`
     - Closure of type ``((SpanData) -> SpanData?)?`` to modify or ignore spans. See :ref:`ios-rum-span-filtering`.
   * - :code:`sessionSamplingRatio`
     - Percentage of sessions to sample. Expressed as a proportion in the range ``0.0`` to ``1.0``. The default value is ``1.0``.
   * - :code:`allowInsecureBeacon`
     - If set to ``true``, this setting allows beacon URLs that use the HTTP protocol. The default value is ``false``.
   * - :code:`enableDiskCache`
     - Activates caching of exported spans. All spans are written to local storage and deleted after a successful export. The default value is ``false``.
   * - :code:`spanDiskCacheMaxSize`
     - Threshold, in megabytes, from which spans start to be dropped from the disk cache. The oldest spans are dropped first. Only applicable when disk caching is activated. The default value is ``25`` megabytes.
   * - :code:`slowRenderingDetectionEnabled`
     - Activates the slow rendering detection feature. The default value is ``false``. See :ref:`ios-slow-rendering-data`.
   * - :code:`slowFrameDetectionThresholdMs`
     - Optional setting that tags as slow all frames that took more than the specified time, in milliseconds. The default value is ``16.7`` milliseconds.
   * - :code:`frozenFrameDetectionThresholdMs`
     - Optional setting that tags as frozen all frames that took more than the specified time, in milliseconds. The default value is ``700`` milliseconds.
   * - :code:`debug`
     - Activates debug logging. The default value is ``false``.

.. _ios-rum-instrumentation-settings:

Instrumentation settings
==============================================

Use the following settings to activate or deactivate the collection of specific data:

.. list-table:: 
   :header-rows: 1
   :widths: 20 80

   * - Option
     - Description
   * - :code:`showVCInstrumentation`
     - Activates the creation of spans for ``ViewController Show`` events. The default value is ``true``.
   * - :code:`screenNameSpans`
     - Activates the creation of spans for changes to the screen name.
   * - :code:`networkInstrumentation`
     - Activates the creation of spans for network activities.