rundeck-healthcheck
====
Tested with Rundeck version >= 2.1.x and Centos 6

Needs
----
- Two Rundeck nodes running in Active/Passive state behind HAProxy 
- All jobs set to run the failed node are moved the current Active node.

What this does
----
- It runs a small daeomon the HAProxy server and queries HAProxy for the node states.  When a node goes down it triggers a failover to the Rundeck API on the Active node.

Perl Dependencies
----
HTTP::Request::Common
LWP
YAML
Proc::Daemon

Configuration
----
A sample rundeck.yml configuration file is offered that can be put into /etc
- you need to replace node1 node2 with the real names of your servers and add the UUID's from each host in your Rundeck cluster.

TODO
----
- State tacking - currently keeps firing failover event until node comes back up.  Not a problem in a two now scenario but it's not targeted and is actually wateful.
- Rundeck doesn't seem (afik) job balancing buit in.  I find that depending on the Load Balancer scheduler jobs are set in the DB to run fron one node or the other.  It is possible to do some sort of balancing.  That is only needed unless they build that in some day.

Other Community offerings for Rundeck Failover
----
The best and only real failover solution that seems to be availalble is the following project on github. 

https://github.com/ahonor/rundeck-vagrant/tree/master/primary-secondary-failover

This is a very complete process but my perspective it involves the tracking of jobs via XML outside of Rundeck.  The tool is complete but I felt like it worked around Rundeck and tries to handle VIP management and I feel like some of those things are already handled in our current environment.  I also wanted to utilize the Rundeck API as much as possible.  Since this didn't quite fit, this simple Perl script was a quick answer to the current problem.  If you're not using HAproxy this seems to be the best tool available and other than issues I had with XMLStartlet, it works just fine and of course it has Vagrant so big thumbs up to the developer.


