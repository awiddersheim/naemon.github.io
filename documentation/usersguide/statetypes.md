---
layout: doctoc
title: State Types
---

{% include review_required.md %}

<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="hostchecks.html">Host Checks</a>, <a href="servicechecks.html">Service Checks</a>, <a href="eventhandlers.html">Event Handlers</a>, <a href="notifications.html">Notifications</a>

### Introduction

The current state of monitored services and hosts is determined by two components:

<ul>
<li>The status of the service or host (i.e. OK, WARNING, UP, DOWN, etc.)</li>
<li>Tye <i>type</i> of state the service or host is in</li>
</ul>

There are two state types in Naemon - SOFT states and HARD states.  These state types are a crucial part of the monitoring logic, as they are used to determine when <a href="eventhandlers.html">event handlers</a> are executed and when <a href="notifications.html">notifications</a> are initially sent out.

This document describes the difference between SOFT and HARD states, how they occur, and what happens when they occur.

### Service and Host Check Retries

In order to prevent false alarms from transient problems, Naemon allows you to define how many times a service or host should be (re)checked before it is considered to have a "real" problem.  This is controlled by the <i>max_check_attempts</i> option in the host and service definitions.  Understanding how hosts and services are (re)checked in order to determine if a real problem exists is important in understanding how state types work.

### Soft States

Soft states occur in the following situations...

<ul>
<li>When a service or host check results in a non-OK or non-UP state and the service check has not yet been (re)checked the number of times specified by the <i>max_check_attempts</i> directive in the service or host definition.  This is called a soft error.
<li>When a service or host recovers from a soft error.  This is considered a soft recovery.
</ul>

The following things occur when hosts or services experience SOFT state changes:

<ul>
<li>The SOFT state is logged.
<li>Event handlers are executed to handle the SOFT state.
</ul>

SOFT states are only logged if you enabled the <a href="configmain.html#log_service_retries">log_service_retries</a> or <a href="configmain.html#log_host_retries">log_host_retries</a> options in your main configuration file.
The only important thing that really happens during a soft state is the execution of event handlers.  Using event handlers can be particularly useful if you want to try and proactively fix a problem before it turns into a HARD state.
The <a href="macrolist.html#hoststatetype">$HOSTSTATETYPE$</a> or <a href="macrolist.html#servicestatetype">$SERVICESTATETYPE$</a> macros will have a value of "<i>SOFT</i>" when event handlers are executed, which allows your event handler scripts to know when they should take corrective action.  More information on event handlers can be found <a href="eventhandlers.html">here</a>.

### Hard States

Hard states occur for hosts and services in the following situations:

<ul>
<li>When a host or service check results in a non-UP or non-OK state and it has been (re)checked the number of times specified by the <i>max_check_attempts</i> option in the host or service definition.  This is a hard error state.
<li>When a host or service transitions from one hard error state to another error state (e.g. WARNING to CRITICAL).</li>
<li>When a service check results in a non-OK state and its corresponding host is either DOWN or UNREACHABLE.
<li>When a host or service recovers from a hard error state.  This is considered to be a hard recovery.
<li>When a <a href="passivechecks.html">passive host check</a> is received. Passive host checks are treated as HARD unless the <a href="configmain.html#passive_host_checks_are_soft">passive_host_checks_are_soft</a> option is enabled.</li>
</ul>

The following things occur when hosts or services experience HARD state changes:

<ul>
<li>The HARD state is logged.
<li>Event handlers are executed to handle the HARD state.
<li>Contacts are notifified of the host or service problem or recovery.
</ul>

The <a href="macrolist.html#hoststatetype">$HOSTSTATETYPE$</a> or <a href="macrolist.html#servicestatetype">$SERVICESTATETYPE$</a> macros will have a value of "<i>HARD</i>" when event handlers are executed, which allows your event handler scripts to know when they should take corrective action.  More information on event handlers can be found <a href="eventhandlers.html">here</a>.

### Example

Here's an example of how state types are determined, when state changes occur, and when event handlers and notifications are sent out.  The table below shows consecutive checks of a service over time.  The service has a <i>max_check_attempts</i> value of 3.

<table border="1" class="Default">
<tr><th>Time</th><th>Check #</th><th>State</th><th>State Type</th><th>State Change</th><th>Notes</th></tr>
<tr><td>0</td><td>1</td><td>OK</td><td>HARD</td><td>No</td><td>Initial state of the service</td></tr>
<tr><td>1</td><td>1</td><td>CRITICAL</td><td>SOFT</td><td>Yes</td><td>First detection of a non-OK state.  Event handlers execute.</td></tr>
<tr><td>2</td><td>2</td><td>WARNING</td><td>SOFT</td><td>Yes</td><td>Service continues to be in a non-OK state.  Event handlers execute.</td></tr>
<tr><td>3</td><td>3</td><td>CRITICAL</td><td>HARD</td><td>Yes</td><td>Max check attempts has been reached, so service goes into a HARD state.  Event handlers execute and a problem notification is sent out.  Check # is reset to 1 immediately after this happens.</td></tr>
<tr><td>4</td><td>1</td><td>WARNING</td><td>HARD</td><td>Yes</td><td>Service changes to a HARD WARNING state.  Event handlers execute and a problem notification is sent out.</td></tr>
<tr><td>5</td><td>1</td><td>WARNING</td><td>HARD</td><td>No</td><td>Service stabilizes in a HARD problem state.  Depending on what the notification interval for the service is, another notification might be sent out.</td></tr>
<tr><td>6</td><td>1</td><td>OK</td><td>HARD</td><td>Yes</td><td>Service experiences a HARD recovery.  Event handlers execute and a recovery notification is sent out.</td></tr>
<tr><td>7</td><td>1</td><td>OK</td><td>HARD</td><td>No</td><td>Service is still OK.</td></tr>
<tr><td>8</td><td>1</td><td>UNKNOWN</td><td>SOFT</td><td>Yes</td><td>Service is detected as changing to a SOFT non-OK state.  Event handlers execute.</td></tr>
<tr><td>9</td><td>2</td><td>OK</td><td>SOFT</td><td>Yes</td><td>Service experiences a SOFT recovery.  Event handlers execute, but notification are not sent, as this wasn't a "real" problem.  State type is set HARD and check # is reset to 1 immediately after this happens.</td></tr>
<tr><td>10</td><td>1</td><td>OK</td><td>HARD</td><td>No</td><td>Service stabilizes in an OK state.</td></tr>
</table>
