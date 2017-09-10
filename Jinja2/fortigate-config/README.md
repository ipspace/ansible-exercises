# Create firewall groups for Fortinet firewalls

This exercise was contributed by [Brandon Dyzel](https://www.linkedin.com/in/brandon-dyzel-a11a9210/)

Using the following data model:

    ---
    vdom: WAN

    fwaddrs:
    - fwaname: HST_SERVERA_10.20.1.1
      fwaint: ZNE_INTERN
      fwaip: 10.20.1.1
    - fwaname: HST_SERVERB_10.20.1.2
      fwaint: ZNE_INTERN
      fwaip: 10.20.1.2
    - fwaname: NET_NETWORKA_10.20.1.0
      fwaint: ZNE_INTERN
      fwaip: 10.20.1.0
      fwasubnt: 255.255.255.0

    fwaddrsgrp:
    - fwgname: ADG_INTERN_GROUP1
      fwgmember:
        HST_SERVERA_10.20.1.1
        NET_NETWORKA_10.20.1.0
    - fwgname: ADG_INTERN_GROUP2
      fwgmember:
        HST_ZNE_SERVERB_10.20.1.2
        NET_ZNE_NETWORKA_10.20.1.0

... create a Jinja2 template that will render the following configuration for a Fortinet firewall:

    config vdom
     edit WAN

     config firewall address
      edit "HST_SERVERA_10.20.1.1"
       set associated-interface "ZNE_INTERN"
       set subnet 10.20.1.1 255.255.255.255
      next
      edit "HST_SERVERB_10.20.1.2"
       set associated-interface "ZNE_INTERN"
       set subnet 10.20.1.2 255.255.255.255
      next
      edit "NET_NETWORKA_10.20.1.0"
       set associated-interface "ZNE_INTERN"
       set subnet 10.20.1.0 255.255.255.0
      next
     next

     config firewall addrgrp
      edit "ADG_INTERN_GROUP1"
       append member "HST_SERVERA_10.20.1.1" "NET_NETWORKA_10.20.1.0"
      next
      edit "ADG_INTERN_GROUP2"
       append member "HST_ZNE_SERVERB_10.20.1.2" "NET_ZNE_NETWORKA_10.20.1.0"
      next
     next

Your template must generate a valid configuration even when the data model does not contain optional elements (see below).

## Data model description

* **vdom** - mandatory - virtual firewall name

* **fwaddrs** - list of object addresses to add (optional)

  * **fwaname** - object name (mandatory)

  * **fwaint*** - interface (mandatory)

  * **fwasubnt** - subnet mask (optional)

* **fwaddrsgrp** - list of address groups to add (optional)

  * **fwgname** - name of address group (mandatory)

  * **fwgmember** - list of members of an address group

## Optional exercise

Create an Ansible playbook that will verify the validity of the data model