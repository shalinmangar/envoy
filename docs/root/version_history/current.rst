1.17.0 (pending)
================

Incompatible Behavior Changes
-----------------------------
*Changes that are expected to cause an incompatibility if applicable; deployment changes are likely required*

Minor Behavior Changes
----------------------
*Changes that may cause incompatibilities for some users, but should not for most*

* build: the Alpine based debug images are no longer built in CI, use Ubuntu based images instead.
* ext_authz filter: the deprecated field :ref:`use_alpha <envoy_api_field_config.filter.http.ext_authz.v2.ExtAuthz.use_alpha>` is no longer supported and cannot be set anymore.

Bug Fixes
---------
*Changes expected to improve the state of the world and are unlikely to have negative effects*

* active health checks: health checks using a TLS transport socket and secrets delivered via :ref:`SDS <config_secret_discovery_service>` will now wait until secrets are loaded before the first health check attempt. This should improve startup times by not having to wait for the :ref:`no_traffic_interval <envoy_v3_api_field_config.core.v3.HealthCheck.no_traffic_interval>` until the next attempt.

* http: sending CONNECT_ERROR for HTTP/2 where appropriate during CONNECT requests.

Removed Config or Runtime
-------------------------
*Normally occurs at the end of the* :ref:`deprecation period <deprecated>`

* ext_authz: removed auto ignore case in HTTP-based `ext_authz` header matching and the runtime guard `envoy.reloadable_features.ext_authz_http_service_enable_case_sensitive_string_matcher`. To ignore case, set the :ref:`ignore_case <envoy_api_field_type.matcher.StringMatcher.ignore_case>` field to true.
* http: flip default HTTP/1 and HTTP/2 server codec implementations to new codecs that remove the use of exceptions for control flow. To revert to old codec behavior, set the runtime feature `envoy.reloadable_features.new_codec_behavior` to false.
* http: removed `envoy.reloadable_features.http1_flood_protection` and legacy code path for turning flood protection off.

New Features
------------
* grpc: implemented header value syntax support when defining :ref:`initial metadata <envoy_v3_api_field_config.core.v3.GrpcService.initial_metadata>` for gRPC-based `ext_authz` :ref:`HTTP <envoy_v3_api_field_extensions.filters.http.ext_authz.v3.ExtAuthz.grpc_service>` and :ref:`network <envoy_v3_api_field_extensions.filters.network.ext_authz.v3.ExtAuthz.grpc_service>` filters, and :ref:`ratelimit <envoy_v3_api_field_config.ratelimit.v3.RateLimitServiceConfig.grpc_service>` filters.
* hds: added support for delta updates in the :ref:`HealthCheckSpecifier <envoy_v3_api_msg_service.health.v3.HealthCheckSpecifier>`, making only the Endpoints and Health Checkers that changed be reconstructed on receiving a new message, rather than the entire HDS.
* health_check: added option to use :ref:`no_traffic_healthy_interval <envoy_v3_api_field_config.core.v3.HealthCheck.no_traffic_healthy_interval>` which allows a different no traffic interval when the host is healthy.
* listener: added an optional :ref:`default filter chain <envoy_v3_api_field_config.listener.v3.Listener.default_filter_chain>`. If this field is supplied, and none of the :ref:`filter_chains <envoy_v3_api_field_config.listener.v3.Listener.filter_chains>` matches, this default filter chain is used to serve the connection.
* mongo_proxy: the list of commands to produce metrics for is now :ref:`configurable <envoy_v3_api_field_extensions.filters.network.mongo_proxy.v3.MongoProxy.commands>`.
* ratelimit: added support for use of various :ref:`metadata <envoy_v3_api_field_config.route.v3.RateLimit.Action.metadata>` as a ratelimit action.
* ratelimit: added :ref:`disable_x_envoy_ratelimited_header <envoy_v3_api_msg_extensions.filters.http.ratelimit.v3.RateLimit>` option to disable `X-Envoy-RateLimited` header.
* tcp: added a new :ref:`envoy.overload_actions.reject_incoming_connections <config_overload_manager_overload_actions>` action to reject incoming TCP connections.

Deprecated
----------
* ratelimit: the :ref:`dynamic metadata <envoy_v3_api_field_config.route.v3.RateLimit.Action.dynamic_metadata>` action is deprecated in favor of the more generic :ref:`metadata <envoy_v3_api_field_config.route.v3.RateLimit.Action.metadata>` action.
