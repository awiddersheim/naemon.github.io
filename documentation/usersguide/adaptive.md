---
layout: doctoc
title: Adaptive Monitoring
---

{% include review_required.md %}

<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="extcommands.html">External Commands</a>

### Introduction

Nagios allows you to change certain commands and host and service check attributes during runtime.  I'll refer to this feature as "adaptive monitoring".  Please note that the adaptive monitoring features found in Nagios will probably not be of much use to 99% of users, but they do allow you to do some neat things.

### What Can Be Changed?

The following service check attributes can be changed during runtime:

* Check command (and command arguments)
* Check interval
* Max check attempts
* Check timeperiod
* Event handler command (and command arguments)

The following host check attributes can be changed during runtime:

* Check command (and command arguments)
* Check interval
* Max check attempts
* Check timeperiod
* Event handler command (and command arguments)

The following global attributes can be changed during runtime:

* Global host event handler command (and command arguments)
* Global service event handler command (and command arguments)

### External Commands For Adaptive Monitoring

In order to change global or host- or service-specific attributes during runtime, you must submit the appropriate <a href="extcommands.html">external command</a> to Nagios via the <a href="configmain.html#command_file">external command file</a>.  The table below lists the different attributes that may be changed during runtime, along with the external command to accomplish the job.

A full listing of external commands that can be used for adaptive monitoring (along with examples of how to use them) can be found online at the following URL: <a href="http://www.nagios.org/developerinfo/externalcommands/" target="_blank">http://www.nagios.org/developerinfo/externalcommands/</a>

<span class="glyphicon glyphicon-pencil"></span> Notes:

* When changing check commands, check timeperiods, or event handler commands, it is important to note that the new values for these options must have been defined before Nagios was started.  Any request to change a command or timeperiod to one which had not been defined when Nagios was started is ignored.
* You can specify command arguments along with the actual command name - just seperate individual arguments from the command name (and from each other) using bang (!) characters.  More information on how arguments in command definitions are processed during runtime can be found in the documentation on <a href="macros.html">macros</a>.
