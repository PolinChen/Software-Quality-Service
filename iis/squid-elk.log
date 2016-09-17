## squid proxy logs


>
Squid Log Files
The logs are a valuable source of information about Squid workloads and performance. The logs record not only access information, but also system configuration errors and resource consumption (e.g. memory, disk space). There are several log file maintained by Squid. Some have to be explicitly activated during compile time, others can safely be deactivated during run-time.

- [Squid Log Files](http://wiki.squid-cache.org/SquidFaq/SquidLogs)


### Squid Error Messages
Error messages come in several forms. Debug traces are not logged at level 0 or level 1. These levels are reserved for important and critical administrative messages.

- FATAL messages indicate a problem which has killed the Squid process. Affecting all current client traffic being supplied by that Squid instance.
 - Obviously if these occur when starting or configuring a Squid component it must be resolved before you can run Squid.
- ERROR messages indicate a serious problem which has broken an individual client transaction and may have some effect on other clients indirectly. But has not completely aborted all traffic service.
 - These can also occur when starting or configuring Squid components. In which case any service actions which that component would have supplied will not happen until it is resolved and Squid reconfigured.
 - NOTE: Some log level 0 error messages inherited from older Squid versions exist without any prioritization tag.
- WARNING messages indicate problems which might be causing problems to the client, but Squid is capable of working around automatically. These usually only display at log level 1 and higher.
 - NOTE: Some log level 1 warning messages inherited from older Squid versions exist without any prioritization tag.
- SECURITY ERROR messages indicate problems processing a client request with the security controls which Squid has been configured with. Some impossible condition is required to pass the security test.
 - This is commonly seen when testing whether to accept a client request based on some reply detail which will only be available in the future.
- SECURITY ALERT messages indicate security attack problems being detected. This is only for problems which are unambiguous. 'Attacks' signatures which can appear in normal traffic are logged as regular WARNING.
 - A complete solution to these usually requires fixing the client, which may not be possible.
 - Administrative workarounds (extra firewall rules etc) can assist Squid in reducing the damage to network performance.
 - Attack notices may seem rather critical, but occur at level 1 since in all cases Squid also has some workaround it can perform.
- SECURITY NOTICE messages can appear during startup and reconfigure to indicate security related problems with the configuration file setting. These are accompanied by hints for better configuration where possible, and an indication of what Squid is going to do instead of the configured action.

