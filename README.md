UPDATED IConrad 

I'll go one-for-one here:

1.Set up a KVM hypervisor.

Yeah, this is as good a choice as any.


2. Inside of that KVM hypervisor, install a Spacewalk server. Use CentOS 6 as the distro for all work below. (For bonus points, set up errata importation on the CentOS channels, so you can properly see security update advisory information.)

Never used it, but this would definitely give you the concepts that can be adapted for RHN or Oracle Linux's repo structure.


3. Create a VM to provide named and dhcpd service to your entire environment. Set up the dhcp daemon to use the Spacewalk server as the pxeboot machine (thus allowing you to use Cobbler to do unattended OS installs). Make sure that every forward zone you create has a reverse zone associated with it. Use something like "internal.virtnet" (but not ".local") as your internal DNS zone.

This is a good basis. An advanced situation would be to know the concepts behind dynamic DNS for the interview, but seriously, don't try building a server at this point.


4. Use that Spacewalk server to automatically (without touching it) install a new pair of OS instances, with which you will then create a Master/Master pair of LDAP servers. Make sure they register with the Spacewalk server. Do not allow anonymous bind, do not use unencrypted LDAP.

Good practice.


5. Reconfigure all 3 servers to use LDAP authentication.

You will absolutely have to do this in your first job. If they don't have it already, you will have the job of trying to make up for someone else's mistake.


6. Create two new VMs, again unattendedly, which will then be Postgresql VMs. Use pgpool-II to set up master/master replication between them. Export the database from your Spacewalk server and import it into the new pgsql cluster. Reconfigure your Spacewalk instance to run off of that server.

Can't comment much on this one, but sadly, the reality is that if you know how to login into the server, you will be drafted as a DBA.


7. Set up a Puppet Master. Plug it into the Spacewalk server for identifying the inventory it will need to work with. (Cheat and use ansible for deployment purposes, again plugging into the Spacewalk server.)

Ansible, salt, or any number of other systems are better. Puppet needs to get onboard with how deployments run. The always live is a killer and you can destroy an environment very quickly if you get this wrong.


8. Deploy another VM. Install iscsitgt and nfs-kernel-server on it. Export a LUN and an NFS share.

Not quite sure about the first of these. It's good for learning the concept, but I question how much industry penetration this has.


9. Deploy another VM. Install bakula on it, using the postgresql cluster to store its database. Register each machine on it, storing to flatfile. Store the bakula VM's image on the iscsi LUN, and every other machine on the NFS share.

bakula is one of the best backup solutions out there. Love it. Good for learning concepts. But, I will state here that the one thing you will most likely be asked to do at some point is a bare-metal backup, which is a complete rebuild of a system. Know bakula's limitations wrt to that and never promise more than it can deliver.

10. Deploy two more VMs. These will have httpd (Apache2) on them. Leave essentially default for now.

Very common.. even in places where they don't actually need it.


11. Deploy two more VMs. These will have tomcat on them. Use JBoss Cache to replicate the session caches between them. Use the httpd servers as the frontends for this. The application you will run is JBoss Wiki.

Very common.. even in places where they don't actually need it.


12. You guessed right, deploy another VM. This will do iptables-based NAT/round-robin loadbalancing between the two httpd servers.

Yeah.


13. Deploy another VM. On this VM, install postfix. Set it up to use a gmail account to allow you to have it send emails, and receive messages only from your internal network.

Also very common.


14. Deploy another VM. On this VM, set up a Nagios server. Have it use snmp to monitor the communication state of every relevant service involved above. This means doing a "is the right port open" check, and a "I got the right kind of response" check and "We still have filesystem space free" check.

Even if you don't use Nagios in your first job, the concepts are great and transferable to other solutions.


15. Deploy another VM. On this VM, set up a syslog daemon to listen to every other server's input. Reconfigure each other server to send their logging output to various files on the syslog server. (For extra credit, set up logstash or kibana or greylog to parse those logs.)

In terms of priorities, this one should be up there around 1 or 2.


16. Document every last step you did in getting to this point in your brand new Wiki.

Yes, get your training on getting fed up with documentation in early..


I'll lump the ones below together.These are good concepts, but overkill if you are just setting out to learn linux. I'd focus more on things like creating users, setting permissions, learning how to do things like "finding what file is filling up /var without accidentally deleting lastlog, which is lying about its size". Have someone break something on your system and leave you to figure out how to fix it. (Like mounting the filesystem read-only, or some such thing).


17. Now go back and create Puppet Manifests to ensure that every last one of these machines is authenticating to the LDAP servers, registered to the Spacewalk server, and backed up by the bakula server.

18. Now go back, reference your documents, and set up a Puppet Razor profile that hooks into each of these things to allow you to recreate, from scratch, each individual server.

19. Destroy every secondary machine you've created and use the above profile to recreate them, joining them to the clusters as needed.

20. Bonus exercise: create three more VMs. A CentOS 5, 6, and 7 machine. On each of these machines, set them up to allow you to create custom RPMs and import them into the Spacewalk server instance. Ensure your Puppet configurations work for all three and produce like-for-like behaviors.


