---
layout: doctoc
title: Object Configuration Overview
---

{% include review_required.md %}

<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="config.html">Configuration Overview</a>, <a href="objectdefinitions.html">Object Definitions</a>

### What Are Objects?

Objects are all the elements that are involved in the monitoring and notification logic.  Types of objects include:

* Services
* Service Groups
* Hosts
* Host Groups
* Contacts
* Contact Groups
* Commands
* Time Periods
* Notification Escalations
* Notification and Execution Dependencies

More information on what objects are and how they relate to each other can be found below.

### Where Are Objects Defined?

Objects can be defined in one or more configuration files and/or directories that you specify using the <a href="configmain.html#cfg_file">cfg_file</a> and/or <a href="configmain.html#cfg_dir">cfg_dir</a> directives in the main configuration file.

<span class="glyphicon glyphicon-thumbs-up"></span> Tip: When you follow <a href="quickstart.html">quickstart installation guide</a>, several sample object configuration files are placed in */usr/local/nagios/etc/objects/*.  You can use these sample files to see how object inheritance works and learn how to define your own object definitions.

### How Are Objects Defined?

Objects are defined in a flexible template format, which can make it much easier to manage your Naemon configuration in the long term.  Basic information on how to define objects in your configuration files can be found <a href="objectdefinitions.html">here</a>.

Once you get familiar with the basics of how to define objects, you should read up on <a href="objectinheritance.html">object inheritance</a>, as it will make your configuration more robust for the future.  Seasoned users can exploit some advanced features of object definitions as described in the documentation on <a href="objecttricks.html">object tricks</a>.

### Objects Explained

Some of the main object types are explained in greater detail below...

<table border="0" width="100%">
<tr>
<td valign="top">
<p>
<a href="objectdefinitions.html#host"><b>Hosts</b></a> are one of the central objects in the monitoring logic.  Important attributes of hosts are as follows:
<ul>
<li>Hosts are usually physical devices on your network (servers, workstations, routers, switches, printers, etc).</li>
<li>Hosts have an address of some kind (e.g. an IP or MAC address).</li>
<li>Hosts have one or more more services associated with them.</li>
<li> Hosts can have parent/child relationships with other hosts, often representing real-world network connections, which is used in the <a href="networkreachability.html">network reachability</a> logic.</li>
</ul>

<a href="objectdefinitions.html#hostgroup"><b>Host Groups</b></a> are groups of one or more hosts.  Host groups can make it easier to (1) view the status of related hosts in the Naemon web interface and (2) simplify your configuration through the use of <a href="objecttricks.html">object tricks</a>.
</td>
<td valign="top" width="100px">
<img src="/images/objects-hosts.png" border="0" alt="Hosts">
</td>
</tr>
</table>


<table border="0" width="100%">
<tr>
<td valign="top">
<a href="objectdefinitions.html#service"><b>Services</b></a> are one of the central objects in the monitoring logic.  Services are associated with hosts and can be:
<ul>
<li>Attributes of a host (CPU load, disk usage, uptime, etc.)</li>
<li>Services provided by the host (HTTP, POP3, FTP, SSH, etc.)</li>
<li>Other things associated with the host (DNS records, etc.)</li>
</ul>

<a href="objectdefinitions.html#servicegroup"><b>Service Groups</b></a> are groups of one or more services.  Service groups can make it easier to (1) view the status of related services in the Naemon web interface and (2) simplify your configuration through the use of <a href="objecttricks.html">object tricks</a>.
</td>
<td valign="top" width="100px">
<img src="/images/objects-services.png" border="0" alt="Services">
</td>
</tr>
</table>

<table border="0" width="100%">
<tr>
<td valign="top">
<a href="objectdefinitions.html#contact"><b>Contacts</b></a> are people involved in the notification process:
<ul>
<li>Contacts have one or more notification methods (cellphone, pager, email, instant messaging, etc.)</li>
<li>Contacts receive notifications for hosts and service they are responsible for
<a href="objectdefinitions.html#contactgroup"><b>Contact Groups</b></a> are groups of one or more contacts.  Contact groups can make it easier to define all the people who get notified when certain host or service problems occur.</li>
</ul>
</td>
<td valign="top" width="100px">
<img src="/images/objects-contacts.png" border="0" alt="Contacts">
</td>
</tr>
</table>

<table border="0" width="100%">
<tr>
<td valign="top">
<a href="objectdefinitions.html#timeperiod"><b>Timeperiods</b></a> are are used to control:
<ul>
<li>When hosts and services can be monitored</li>
<li>When contacts can receive notifications</li>
</ul>
Information on how timeperiods work can be found <a href="timeperiods.html">here</a>.
</td>
<td valign="top" width="100px">
<img src="/images/objects-timeperiods.png" border="0" alt="Timeperiods">
</td>
</tr>
</table>

<table border="0" width="100%">
<tr>
<td valign="top">
<a href="objectdefinitions.html#command"><b>Commands</b></a> are used to tell Naemon what programs, scripts, etc. it should execute to perform:
<ul>
<li> Host and service checks
<li>Notifications</li>
<li>Event handlers</li>
<li>and more...</li>
</ul>
</td>
<td valign="top" width="100px">
<img src="/images/objects-commands.png" border="0" alt="Commands">
</td>
</tr>
</table>
