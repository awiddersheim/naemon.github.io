---
layout: doctoc
title: Configuration Overview
---

{% include review_required.md %}


<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="configmain.html">Main Configuration File</a>, <a href="configobject.html">Object Configuration Overview</a>, <a href="configcgi.html">CGI Configuration File</a>

### Introduction

There are several different configuration files that you're going to need to create or edit before you start monitoring anything.  Be patient!  Configuring Naemon can take quite a while, especially if you're first-time user.  Once you figure out how things work, it'll all be well worth your time. :-)

<span class="glyphicon glyphicon-pencil"></span> Note: Sample configuration files are installed in the */usr/local/nagios/etc/* directory when you follow the <a href="quickstart.html">quickstart installation guide</a>.

<img src="/images/configoverview.png" border="0" style="float: right" alt="Config Overview" title="Config Overview">

### Main Configuration File

The main configuration file contains a number of directives that affect how the Naemon daemon operates.  This config file is read by both the Naemon daemon and the CGIs.  This is where you're going to want to get started in your configuration adventures.

Documentation for the main configuration file can be found <a href="configmain.html">here</a>.

### Resource File(s)

Resource files can be used to store user-defined macros.  The main point of having resource files is to use them to store sensitive configuration information (like passwords), without making them available to the CGIs.

You can specify one or more optional resource files by using the <a href="configmain.html#resource_file">resource_file</a> directive in your main configuration file.

### Object Definition Files

Object definition files are used to define hosts, services, hostgroups, contacts, contactgroups, commands, etc.  This is where you define all the things you want monitor and how you want to monitor them.

You can specify one or more object definition files by using the <a href="configmain.html#cfg_file">cfg_file</a> and/or <a href="configmain.html#cfg_dir">cfg_dir</a> directives in your main configuration file.

An introduction to object definitions, and how they relate to each other, can be found <a href="configobject.html">here</a>.

### CGI Configuration File

The CGI configuration file contains a number of directives that affect the operation of the <a href="cgis.html">CGIs</a>.  It also contains a reference the main configuration file, so the CGIs know how you've configured Naemon and where your object defintions are stored.

Documentation for the CGI configuration file can be found <a href="configcgi.html">here</a>.
