======================
Sunsetted Technologies
======================

There are a number of technologies and components that have been
*sunsetted* or *deprecated*. Commonly a sunsetted technology has been
deprecated in favor of a newer choice.

Technologies and software listed below has been sunsetted, and
references to it, in code or prose within the platform, are generally
the results of lack of time to fully purge them.

When working in an area containing references to any mentioned
technologies, developers are encouraged to assist in removing them and
replacing them with the new preferred equivalent.

Some reasons for each deprecation are included below.


CPython
-------

Use of the CPython interpreter has generally been deprecated in the
platform. New projects are encouraged to use **`PyPy`**, and existing
projects are encouraged to move to PyPy if they are compute-intensive.

Explanation
############

* Speed, performance, and resultant cost efficiency
* Minimal "porting" effort

Chef
----

`Chef <https://www.chef.io/chef/get-chef/>`_ was previously used as
the configuration management system for platform components (and
otherwise). **Ansible** should be used for all new services, and
existing components' recipes should be ported to unified Ansible
cookbooks in coordination with devops.

Explanation
############

* Written in a language closer to the day-to-day of engineering (Python vs.
  Ruby)
* Can run agent-less on infrastructure machines, resulting in less resource
  usage
* Chef cookbooks were poorly maintained and needed revision regardless

Vagrant
-------

Use of `vagrant <https://www.vagrantup.com/>`_ is discouraged for new
and old platform components. Developers are *strongly encouraged* to
make applications runnable **completely locally without expensive,
hard-to-install dependencies**.

**Docker** and **docker-compose** are other alternatives for developing
reproduceable, pre-packaged app stacks.


Explanation
############

* Poor backwards compatibility, and general "version hell" when setting up
  vagrant on new developer machines
* Poor story for provisioning VMs -- Berkshelf was previously used to inject
  cookbooks into VMs for use by `Chef`, but is similarly deprecated and
  discouraged. The berkshelf plugin for vagrant was a consistent source of
  frustration due to the above compatibility concerns.
* Encourages better, self-contained applications for faster development
* VMs were not in-sync with Chef cookbooks used for deployment to real
  machines, leading to multiple divergent provisioning scripts
