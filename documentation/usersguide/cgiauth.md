---
layout: doctoc
title: Authentication And Authorization In The CGIs
---

{% include review_required.md %}

<span class="glyphicon glyphicon-arrow-right"></span> See Also: <a href="configcgi.html">CGI Configuration File Options</a>, <a href="cgis.html">Information on the CGIs</a>

* Issuing Naemon process commands via the <a href="cgis.html#cmd_cgi">command CGI</a>
* Viewing host group, contact, contact group, time period, and command definitions via the <a href="cgis.html#config_cgi">configuration CGI</a>

You will undoubtably want to access this information, so you'll have to assign additional rights for yourself (and possibly other users) as described below...

### Granting Additional Permissions To CGI Information

You can grant *authenticated contacts* or other *authenticated users* permission to additional information in the CGIs by adding them to various authorization variables in the <a href="configcgi.html">CGI configuration file</a>.  I realize that the available options don't allow for getting really specific about particular permissions, but its better than nothing..

Additional authorization can be given to users by adding them to the following variables in the CGI configuration file...

* <a href="configcgi.html#authorized_for_system_information">authorized_for_system_information</a>
* <a href="configcgi.html#authorized_for_system_commands">authorized_for_system_commands</a>
* <a href="configcgi.html#authorized_for_configuration_information">authorized_for_configuration_information</a>
* <a href="configcgi.html#authorized_for_all_hosts">authorized_for_all_hosts</a>
* <a href="configcgi.html#authorized_for_all_host_commands">authorized_for_all_host_commands</a>
* <a href="configcgi.html#authorized_for_all_services">authorized_for_all_services</a>
* <a href="configcgi.html#authorized_for_all_service_commands">authorized_for_all_service_commands</a>

### CGI Authorization Requirements

If you are confused about the authorization needed to access various information in the CGIs, read the *<b>Authorization Requirements</b>* section for each CGI as described <a href="cgis.html">here</a>.

### Authentication On Secured Web Servers

If your web server is located in a secure domain (i.e., behind a firewall) or if you are using SSL, you can define a default username that can be used to access the CGIs.  This is done by defining the <a href="configcgi.html#default_user_name">default_user_name</a> option in the <a href="configcgi.html">CGI configuration file</a>.  By defining a default username that can access the CGIs, you can allow users to access the CGIs without necessarily having to authenticate to the web server.   You may want to use this to avoid having to use basic web authentication, as basic authentication transmits passwords in clear text over the Internet.

**Important:**  Do *not* define a default username unless you are running a secure web server and are sure that everyone who has access to the CGIs has been authenticated in some manner.  If you define this variable, anyone who has not authenticated to the web server will inherit all rights you assign to this user!
