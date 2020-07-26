##### Some newer comments below

COMMENT
This all is very enterprise – i.e., outdated garbage that anyone with a pulse wants to get away from desperately. Use CentOS 8. Learn systemd. Learn how to do all of the above in docker / kubernetes / AWS / GCP. Learn how to deploy nodejs, python and other languages that aren't Java. Learn how to use modern filesystems, both for local use (ZFS, Btrfs) and clustered setups (Gluster/Ceph).


COMMENT
- Haproxy for loadbalancing or corosync/pacemaker for more complex cases

COMMENT
- As a Senior Admin I would be more interested in your ability to bring new technologies and infrastructure to the table. Hashicorp, Ansible, Kubernetes are tools that have great futures and lots of support. Familiarize yourself with Virtualization/Cloud technologies. Don't focus on one Distro. You will use Debian and Red Hat based servers depending on the use case or organization.

- Document everything! You will come across the same issues repeatedly over the years and having the solution in your notes will save a lot of time.

- Understand logging and how to consolidate that info and filter it. You should know how to answer just about any question about a server with a few commands. Learn all the main sh commands.

- Install and configure everything you come across. This is something that has made me a superstar contractor. People know that I can install, configure and support just about anything mostly because as soon as a new app comes across my desk I set it up in a container or VM.

COMMENT
- Yep, also a senior (infrastructure) engineer here. The OP list might have had some value 5-8 years ago, but most of those tools and paradigms have been replaced by containerization and cloud-centric automation. When hiring, I don't ask if you know how to reconfigure LDAP or spacewalk or NFS. I ask if you can implement infrastructure as code, kubernetes RBAC, and how you juggle multiple AWS accounts, and what scripting language you don't suck at. If you can demonstrate proficiency in those types of tasks, I can reasonably trust you to learn how to do LDAP, NFS, or server provisioning pretty quickly if those are ever tasks we need performed.

COMMENT
Some adjustments I'd do:
-CentOS 7 instead of CentOS 6 (mostly because the majority is still on rhel7 / centos7)*
- Foreman-Katello instead of Spacewalk
- Run the dhcpd either from Katello or FreeIPA. Bonus Points for setting up HA on 2 separate VMs ... because why not, I guess.
- FreeIPA instead of OpenLDAP
- Use Ansible for all your configuration Management
- Setup a Server that hosts AWX so you can have Tower-like functionality with ansible
- Bonus Points for a gitlab instance you host yourself for your IAC
- Skip Postgres Cluster for Spacewalk
- Host your own Mailserver dedicated to only that environment and configure all servers to use this server for mail transfers
- For Application Servers, I'd checkout wildfly since you can't download Jboss directly afaik
- In terms of Loadbalancing, I'd go with Traefik or Nginx instead of iptables

* Once done with all the tasks, migrate the servers group by group to CentOS 8

I'm sure there's a lot of stuff I've left out but that's just a few thoughts. Feel free to correct me if I've written some major garbage :D
