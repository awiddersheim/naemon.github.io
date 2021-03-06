---
layout: doctoc
title: Time-Saving Tricks For Object Definitions
---

{% include review_required.md %}

<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="objectdefinitions.html">Object Configuration</a>, <a href="objectinheritance.html">Object Inheritance</a>

### Introduction

This documentation attempts to explain how you can exploit the (somewhat) hidden features of <a href="objectdefinitions.html">template-based object definitions</a> to save your sanity.  How so, you ask?  Several types of objects allow you to specify multiple host names and/or hostgroup names in definitions, allowing you to "copy" the object defintion to multiple hosts or services.  I'll cover each type of object that supports these features seperately.  For starters, the object types which support this time-saving feature are as follows:

<ul>
<li><a href="#service">Services</a>
<li><a href="#serviceescalation">Service escalations</a>
<li><a href="#servicedependency">Service dependencies</a>
<li><a href="#hostescalation">Host escalations</a>
<li><a href="#hostdependency">Host dependencies</a>
<li><a href="#hostgroup">Hostgroups</a>
</ul>

Object types that are not listed above (i.e. timeperiods, commands, etc.) do not support the features I'm about to describe.

### Regular Expression Matching

The examples I give below use "standard" matching of object names
  and <b>*require*</b> <a href="configmain.html#use_regexp_matching">use_regexp_matching</a> to be <b>*disabled*</b>.

If you wish, you can enable regular expression matching for object names by using the <a href="configmain.html#use_regexp_matching">use_regexp_matching</a> config option.  By default, regular expression matching will only be used in object names that contain <b>*</b>, <b>?</b>, <b>+</b>, or <b>\.</b>.  If you want regular expression matching to be used on all object names, enable the <a href="configmain.html#use_true_regexp_matching">use_true_regexp_matching</a> config option.  Regular expressions can be used in any of the fields used in the examples below (host names, hostgroup names, service names, and servicegroup names).

<span class="glyphicon glyphicon-pencil"></span> Note: Be careful when enabling regular expression matching - you may have to change your config file, since some directives that you might not want to be interpreted as a regular expression just might be!  Any problems should become evident once you verify your configuration.

<a name="service"></a>

### Service Definitions

<b>Multiple Hosts:</b><br> If you want to create identical <a href="objectdefinitions.html#service">services</a> that are assigned to multiple hosts, you can specify multiple hosts in the <i>host_name</i> directive.  The definition below would create a service called <i>SOMESERVICE</i> on hosts <i>HOST1</i> through <i>HOSTN</i>.  All the instances of the <i>SOMESERVICE</i> service would be identical (i.e. have the same check command, max check attempts, notification period, etc.).

<pre>
	define <i>service</i>{
		<font color="red">host_name		<i>HOST1,HOST2,HOST3,...,HOSTN</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other service directives</i> ...
		}
</pre>


<b>All Hosts In Multiple Hostgroups:</b><br>If you want to create identical services that are assigned to all hosts in one or more hostgroups, you can do so by creating a single service definition.  How?  The <i>hostgroup_name</i> directive allows you to specify the name of one or more hostgroups that the service should be created for.  The definition below would create a service called <i>SOMESERVICE</i> on all hosts that are members of hostgroups <i>HOSTGROUP1</i> through <i>HOSTGROUPN</i>.  All the instances of the <i>SOMESERVICE</i> service would be identical (i.e. have the same check command, max check attempts, notification period, etc.).

<pre>
	define <i>service</i>{
		<font color="red">hostgroup_name		<i>HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other service directives</i> ...
		}
</pre>

<b>All Hosts:</b><br> If you want to create identical services that are assigned to all hosts that are defined in your configuration files, you can use a wildcard in the <i>host_name</i> directive.  The definition below would create a service called <i>SOMESERVICE</i> on <b>all hosts</b> that are defined in your configuration files.  All the instances of the <i>SOMESERVICE</i> service would be identical (i.e. have the same check command, max check attempts, notification period, etc.).

<pre>
	define <i>service</i>{
		<font color="red">host_name		<i>*</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other service directives</i> ...
		}
</pre>

<b>Excluding Hosts:</b><br>If you want to create identical services on numerous hosts or hostgroups, but would like to exclude some hosts from the definition, this can be accomplished by preceding the host or hostgroup with a ! symbol.

<pre>
	define <i>service</i>{
		<font color="red">host_name             <i>HOST1,HOST2,!HOST3,!HOST4,...,HOSTN</i></font>
		<font color="red">hostgroup_name                <i>HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN</i></font>
		<font color="red">service_description   <i>SOMESERVICE</i></font>
		<i>other service directives</i> ...
		}
</pre>

<a name="serviceescalation"></a>

### Service Escalation Definitions

<b>Multiple Hosts:</b><br> If you want to create <a href="objectdefinitions.html#serviceescalation">service escalations</a> for services of the same name/description that are assigned to multiple hosts, you can specify multiple hosts in the <i>host_name</i> directive.  The definition below would create a service escalation for services called <i>SOMESERVICE</i> on hosts <i>HOST1</i> through <i>HOSTN</i>.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>serviceescalation</i>{
		<font color="red">host_name		<i>HOST1,HOST2,HOST3,...,HOSTN</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other escalation directives</i> ...
		}
</pre>


<b>All Hosts In Multiple Hostgroups:</b><br> If you want to create service escalations for services of the same name/description that are assigned to all hosts in in one or more hostgroups, you can do use the <i>hostgroup_name</i> directive.  The definition below would create a service escalation for services called <i>SOMESERVICE</i> on all hosts that are members of hostgroups <i>HOSTGROUP1</i> through <i>HOSTGROUPN</i>.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>serviceescalation</i>{
		<font color="red">hostgroup_name		<i>HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>All Hosts:</b><br> If you want to create identical service escalations for services of the same name/description that are assigned to all hosts that are defined in your configuration files, you can use a wildcard in the <i>host_name</i> directive.  The definition below would create a service escalation for all services called <i>SOMESERVICE</i> on <b>all hosts</b> that are defined in your configuration files.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>serviceescalation</i>{
		<font color="red">host_name		<i>*</i></font>
		<font color="red">service_description	<i>SOMESERVICE</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>Excluding Hosts:</b><br>If you want to create identical services escalations for services on numerous hosts or hostgroups,
but would like to exclude some hosts from the definition, this can be accomplished by preceding the host or hostgroup with a ! symbol.

<pre>
	define <i>serviceescalation</i>{
		<font color="red">host_name             <i>HOST1,HOST2,!HOST3,!HOST4,...,HOSTN</i></font>
		<font color="red">hostgroup_name                <i>HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN</i></font>
		<font color="red">service_description   <i>SOMESERVICE</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>All Services On Same Host:</b><br> If you want to create <a href="objectdefinitions.html#serviceescalation">service escalations</a> for all services assigned to a particular host, you can use a wildcard in the <i>service_description</i> directive.  The definition below would create a service escalation for <b>all</b> services on host <i>HOST1</i>.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

If you feel like being particularly adventurous, you can specify a wildcard in both the <i>host_name</i> and <i>service_description</i> directives.  Doing so would create a service escalation for <b>all services</b> that you've defined in your configuration files.

<pre>
	define <i>serviceescalation</i>{
		<font color="red">host_name		<i>HOST1</i></font>
		<font color="red">service_description	<i>*</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>Multiple Services On Same Host:</b><br> If you want to create <a href="objectdefinitions.html#serviceescalation">service escalations</a> for all multiple services assigned to a particular host, you can use a specify more than one service description in the <i>service_description</i> directive.  The definition below would create a service escalation for services <i>SERVICE1</i> through <i>SERVICEN</i> on host <i>HOST1</i>.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>serviceescalation</i>{
		<font color="red">host_name		<i>HOST1</i></font>
		<font color="red">service_description	<i>SERVICE1,SERVICE2,...,SERVICEN</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>All Services In Multiple Servicegroups:</b><br> If you want to create service escalations for all services that belong in one or more servicegroups, you can do use the <i>servicegroup_name</i> directive.  The definition below would create service escalations for all services that are members of servicegroups <i>SERVICEGROUP1</i> through <i>SERVICEGROUPN</i>.  All the instances of the service escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>serviceescalation</i>{
		<font color="red">servicegroup_name		<i>SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<a name="servicedependency"></a>

### Service Dependency Definitions

<b>Multiple Hosts:</b><br> If you want to create <a href="objectdefinitions.html#servicedependency">service dependencies</a> for services of the same name/description that are assigned to multiple hosts, you can specify multiple hosts in the <i>host_name</i> and or <i>dependent_host_name</i> directives.  In the example below, service <i>SERVICE2</i> on hosts <i>HOST3</i> and <i>HOST4</i> would be dependent on service <i>SERVICE1</i> on hosts <i>HOST1</i> and <i>HOST2</i>.  All the instances of the service dependencies would be identical except for the host names (i.e. have the same notification failure criteria, etc.).

<pre>
	define <i>servicedependency</i>{
		<font color="red">host_name			<i>HOST1,HOST2</i></font>
		<font color="red">service_description		<i>SERVICE1</i></font>
		<font color="red">dependent_host_name		<i>HOST3,HOST4</i></font>
		<font color="red">dependent_service_description	<i>SERVICE2</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<b>All Hosts In Multiple Hostgroups:</b><br> If you want to create service dependencies for services of the same name/description that are assigned to all hosts in in one or more hostgroups, you can do use the <i>hostgroup_name</i> and/or <i>dependent_hostgroup_name</i> directives.  In the example below, service <i>SERVICE2</i> on all hosts in hostgroups <i>HOSTGROUP3</i> and <i>HOSTGROUP4</i> would be dependent on service <i>SERVICE1</i> on all hosts in hostgroups <i>HOSTGROUP1</i> and <i>HOSTGROUP2</i>.  Assuming there were five hosts in each of the hostgroups, this definition would be equivalent to creating 100 single service dependency definitions!  All the instances of the service dependency would be identical except for the host names (i.e. have the same notification failure criteria, etc.).

<pre>
	define <i>servicedependency</i>{
		<font color="red">hostgroup_name			<i>HOSTGROUP1,HOSTGROUP2</i></font>
		<font color="red">service_description		<i>SERVICE1</i></font>
		<font color="red">dependent_hostgroup_name	<i>HOSTGROUP3,HOSTGROUP4</i></font>
		<font color="red">dependent_service_description	<i>SERVICE2</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<b>All Services On A Host:</b><br> If you want to create service dependencies for all services assigned to a particular host, you can use a wildcard in the <i>service_description</i> and/or <i>dependent_service_description</i> directives.  In the example below, <b>all services</b> on host <i>HOST2</i> would be dependent on <b>all services</b> on host <i>HOST1</i>.  All the instances of the service dependencies would be identical (i.e. have the same notification failure criteria, etc.).

<pre>
	define <i>servicedependency</i>{
		<font color="red">host_name			<i>HOST1</i></font>
		<font color="red">service_description		<i>*</i></font>
		<font color="red">dependent_host_name		<i>HOST2</i></font>
		<font color="red">dependent_service_description	<i>*</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<b>Multiple Services On A Host:</b><br> If you want to create service dependencies for multiple services assigned to a particular host, you can specify more than one service description in the <i>service_description</i> and/or <i>dependent_service_description</i> directives as follows:

<pre>
	define <i>servicedependency</i>{
		<font color="red">host_name			<i>HOST1</i></font>
		<font color="red">service_description		<i>SERVICE1,SERVICE2,...,SERVICEN</i></font>
		<font color="red">dependent_host_name		<i>HOST2</i></font>
		<font color="red">dependent_service_description	<i>SERVICE1,SERVICE2,...,SERVICEN</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<b>All Services In Multiple Servicegroups:</b><br> If you want to create service dependencies for all services that belong in one or more servicegroups, you can do use the <i>servicegroup_name</i> and/or <i>dependent_servicegroup_name</i> directive as follows:

<pre>
	define <i>servicedependency</i>{
		<font color="red">servicegroup_name		<i>SERVICEGROUP1,SERVICEGROUP2,...,SERVICEGROUPN</i></font>
		<font color="red">dependent_servicegroup_name	<i>SERVICEGROUP3,SERVICEGROUP4,...SERVICEGROUPN</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<a name="same_host_dependency"></a>

<b>Same Host Dependencies:</b><br> If you want to create service dependencies for multiple services that are dependent on services on the same host, leave the <i>dependent_host_name</i> and <i>dependent_hostgroup_name</i> directives empty.  The example below assumes that hosts <i>HOST1</i> and <i>HOST2</i> have at least the following four services associated with them: <i>SERVICE1</i>, <i>SERVICE2</i>, <i>SERVICE3</i>, and <i>SERVICE4</i>.  In this example, <i>SERVICE3</i> and <i>SERVICE4</i> on <i>HOST1</i> will be dependent on both <i>SERVICE1</i> and <i>SERVICE2</i> on <i>HOST1</i>.  Similiarly, <i>SERVICE3</i> and <i>SERVICE4</i> on <i>HOST2</i> will be dependent on both <i>SERVICE1</i> and <i>SERVICE2</i> on <i>HOST2</i>.

<pre>
	define <i>servicedependency</i>{
		<font color="red">host_name			<i>HOST1,HOST2</i></font>
		<font color="red">service_description		<i>SERVICE1,SERVICE2</i></font>
		<font color="red">dependent_service_description	<i>SERVICE3,SERVICE4</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<a name="hostescalation"></a>

### Host Escalation Definitions

<b>Multiple Hosts:</b><br> If you want to create <a href="objectdefinitions.html#hostescalation">host escalations</a> for  multiple hosts, you can specify multiple hosts in the <i>host_name</i> directive.  The definition below would create a host escalation for hosts <i>HOST1</i> through <i>HOSTN</i>.  All the instances of the host escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>hostescalation</i>{
		<font color="red">host_name		<i>HOST1,HOST2,HOST3,...,HOSTN</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>All Hosts In Multiple Hostgroups:</b><br> If you want to create host escalations for all hosts in in one or more hostgroups, you can do use the <i>hostgroup_name</i> directive.  The definition below would create a host escalation on all hosts that are members of hostgroups <i>HOSTGROUP1</i> through <i>HOSTGROUPN</i>.  All the instances of the host escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>hostescalation</i>{
		<font color="red">hostgroup_name		<i>HOSTGROUP1,HOSTGROUP2,...,HOSTGROUPN</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>All Hosts:</b><br> If you want to create identical host escalations for all hosts that are defined in your configuration files, you can use a wildcard in the <i>host_name</i> directive.  The definition below would create a hosts escalation for <b>all hosts</b> that are defined in your configuration files.  All the instances of the host escalation would be identical (i.e. have the same contact groups, notification interval, etc.).

<pre>
	define <i>hostescalation</i>{
		<font color="red">host_name		<i>*</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<b>Excluding Hosts:</b><br>If you want to create identical host escalations on numerous hosts or hostgroups, but would like to
 exclude some hosts from the definition, this can be accomplished by preceding the host or hostgroup with a ! symbol.

<pre>
	define <i>hostescalation</i>{
		<font color="red">host_name             <i>HOST1,HOST2,!HOST3,!HOST4,...,HOSTN</i></font>
		<font color="red">hostgroup_name                <i>HOSTGROUP1,HOSTGROUP2,!HOSTGROUP3,!HOSTGROUP4,...,HOSTGROUPN</i></font>
		<i>other escalation directives</i> ...
		}
</pre>

<a name="hostdependency"></a>

### Host Dependency Definitions

<b>Multiple Hosts:</b><br> If you want to create <a href="objectdefinitions.html#hostdependency">host dependencies</a> for  multiple hosts, you can specify multiple hosts in the <i>host_name</i> and/or <i>dependent_host_name</i> directives. The definition below would be equivalent to creating six seperate host dependencies.   In the example above, hosts <i>HOST3</i>, <i>HOST4</i> and <i>HOST5</i> would be dependent upon both <i>HOST1</i> and <i>HOST2</i>.  All the instances of the host dependencies would be identical except for the host names (i.e. have the same notification failure criteria, etc.).

<pre>
	define <i>hostdependency</i>{
		<font color="red">host_name		<i>HOST1,HOST2</i></font>
		<font color="red">dependent_host_name	<i>HOST3,HOST4,HOST5</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<b>All Hosts In Multiple Hostgroups:</b><br> If you want to create host escalations for all hosts in in one or more hostgroups, you can do use the <i>hostgroup_name</i> and /or <i>dependent_hostgroup_name</i> directives.  In the example below, all hosts in hostgroups <i>HOSTGROUP3</i> and <i>HOSTGROUP4</i> would be dependent on all hosts in hostgroups <i>HOSTGROUP1</i> and <i>HOSTGROUP2</i>.  All the instances of the host dependencies would be identical except for  host names (i.e. have the same notification failure criteria, etc.).

<pre>
	define <i>hostdependency</i>{
		<font color="red">hostgroup_name			<i>HOSTGROUP1,HOSTGROUP2</i></font>
		<font color="red">dependent_hostgroup_name	<i>HOSTGROUP3,HOSTGROUP4</i></font>
		<i>other dependency directives</i> ...
		}
</pre>

<a name="hostgroup"></a>

### Hostgroups

<b>All Hosts:</b><br> If you want to create a hostgroup that has all hosts that are defined in your configuration files as members, you can use a wildcard in the <i>members</i> directive.  The definition below would create a hostgroup called <i>HOSTGROUP1</i> that has <b>all hosts</b> that are defined in your configuration files as members.

<pre>
	define <i>hostgroup</i>{
		<i>hostgroup_name</i>		<i>HOSTGROUP1</i>
		<font color="red">members			<i>*</i></font>
		<i>other hostgroup directives</i> ...
		}
</pre>
