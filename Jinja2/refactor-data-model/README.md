# Refactor router data model

This exercise was contributed by [Carl Buchmann](https://www.linkedin.com/in/carl-buchmann-6b436727/)

You inherited a network automation application that generates Cisco IOS router configurations from YAML documents (each YAML document describes one router).

Browsing through the data model and Jinja2 template you discovered data duplication in the data model. For example, a mandatory variable **ip_helper** specifies whether the IP Helper Address functionality is enabled on an interface while a separate optional variable **ip_helper_addresses** specifies the helper IP addresses.

Refactor the application by:

* Removing all redundancies from the data model;
* Further optimizing the data model if needed;
* Rewriting Jinja2 template to generate the same device configurations from refactored data model.