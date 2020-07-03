#### u-IConrad-response
SysAdmin learning

##### This content is from a response to a reddit post from several years ago that outlines the steps to take to learn skills that are used on the job, day to day as opposed to a more high level school type learning track.
The post can be found [here](https://www.reddit.com/r/linuxadmin/comments/2s924h/how_did_you_get_your_start/cnnw1ma/) and also the content is listed below, including some of the responses on the thread that add useful additional points.
##### The original IConrad comment is 5 years old and some items are out of date. 

#### A relevant comment on this post: 
#### "Do everything on centos 7, replace spacewalk and puppetmaster with foreman, use freeipa to do ldap, use an elk stack to do centralized logs, dont worry about centos 5, maybe throw an awx ansible server in there and automate it that way as well.
#### Edit: - I used iconrads original list to go from being a bum to a system engineer within a couple years. It works."

This is what I tell people to do, who ask me "how do I learn to be a Linux sysadmin?".
1. Set up a KVM hypervisor.
2. Inside of that KVM hypervisor, install a Spacewalk server. Use CentOS 6 as the distro for all work below. (For bonus points, set up errata importation on the CentOS channels, so you can properly see security update advisory information)
3. Create a VM to provide named and dhcpd service to your entire environment. Set up the dhcp daemon to use the ~~Spacewalk~~ Foreman server as the pxeboot machine (thus allowing you to use Cobbler to do unattended OS installs). Make sure that every forward zone you create has a reverse zone associated with it. Use something like "internal.virtnet" (but not ".local") as your internal DNS zone.
4. Use that ~~Spacewalk~~ Foreman server to automatically (without touching it) install a new pair of OS instances, with which you will then create a Master/Master pair of LDAP servers. Make sure they register with the Spacewalk server. Do not allow anonymous bind, do not use unencrypted LDAP.
5. Reconfigure all 3 servers to use LDAP authentication.
6. Create two new VMs, again unattendedly, which will then be Postgresql VMs. Use pgpool-II to set up master/master replication between them. Export the database from your Spacewalk server and import it into the new pgsql cluster. Reconfigure your Spacewalk instance to run off of that server.
7. Set up a Puppet Master. Plug it into the Spacewalk server for identifying the inventory it will need to work with. (Cheat and use ansible for deployment purposes, again plugging into the Spacewalk server.)
8. Deploy another VM. Install iscsitgt and nfs-kernel-server on it. Export a LUN and an NFS share.
9. Deploy another VM. Install bakula on it, using the postgresql cluster to store its database. Register each machine on it, storing to flatfile. Store the bakula VM's image on the iscsi LUN, and every other machine on the NFS share.
10. Deploy two more VMs. These will have httpd (Apache2) on them. Leave essentially default for now.
11. Deploy two more VMs. These will have tomcat on them. Use JBoss Cache to replicate the session caches between them. Use the httpd servers as the frontends for this. The application you will run is JBoss Wiki.
12. You guessed right, deploy another VM. This will do iptables-based NAT/round-robin loadbalancing between the two httpd servers.
13. Deploy another VM. On this VM, install postfix. Set it up to use a gmail account to allow you to have it send emails, and receive messages only from your internal network.
14. Deploy another VM. On this VM, set up a Nagios server. Have it use snmp to monitor the communication state of every relevant service involved above. This means doing a "is the right port open" check, and a "I got the right kind of response" check and "We still have filesystem space free" check.
15. Deploy another VM. On this VM, set up a syslog daemon to listen to every other server's input. Reconfigure each other server to send their logging output to various files on the syslog server. (For extra credit, set up logstash or kibana or greylog to parse those logs.)
16. Document every last step you did in getting to this point in your brand new Wiki.
17. Now go back and create Puppet Manifests to ensure that every last one of these machines is authenticating to the LDAP servers, registered to the Spacewalk server, and backed up by the bakula server.
18. Now go back, reference your documents, and set up a Puppet Razor profile that hooks into each of these things to allow you to recreate, from scratch, each individual server.
19. Destroy every secondary machine you've created and use the above profile to recreate them, joining them to the clusters as needed.
20. Bonus exercise: create three more VMs. A CentOS 5, 6, and 7 machine. On each of these machines, set them up to allow you to create custom RPMs and import them into the Spacewalk server instance. Ensure your Puppet configurations work for all three and produce like-for-like behaviors.

Do these things and you will be fully exposed to every aspect of Linux Enterprise systems administration. Do them well and you will have the technical expertise required to seek "Senior" roles. If you go whole-hog crash-course full-time it with no other means of income, I would expect it would take between 3 and 6 months to go from "I think I'm good with computers" to achieving all of these -- assuming you're not afraid of IRC and google (and have neither friends nor family ...).

There will be edits to this comment as I think of relevant details to add.

#### More from IConrad
Honestly the whole thing was meant to expose you at least once to important elements of the trade. I was very honest when I said that if you did every last item on this list then you would be eminently qualified to work any Linux admin posting you might ever encounter.

As to how long these things might take... Each step could conceivably take a person a month to work out if they were only hobbyist/idling through it. Some, if half-assed, would take less time. You could use dnsmasq rather than named/dhcpd, for example.

You could also do away with the Spacewalk server altogether but then you'd have a harder road to haul on getting unattended installs and server inventorying set up. The one thing you could do is follow walk throughs for each item and keep each project's IRC channel open when working on it. ( Or even just idling in them when watching TV or the like. )

The one thing that will do you well however is that when it comes time to landing your first gig, you could literally list this setup on your resume as a qualification.

I will mention in addition that I included tasks that are meant to expose you to enterprise-grade infrastructural architecture but I didn't explain the concepts or reasoning behind them. Part of that was intentional. I believe that people who really want this gig are the ones who would be able to find out about those things and grok it even if they don't know the words, and I'm just elitist enough that I don't want to ever work with people whose sole skill is following howtos like parrots singing. So I'm leaving some stuff out.

I will reemphasize that this list is representative of the actual trade. I've done -- or am doing -- everything on the list. I've corroborated the representativeness of the list with dozens of fellow admins.

#### Another response from IConrad
Actually, RHEL is the most common, but this is for people looking to learn how to be enterprise admins. I'm assuming they're not gonna want to pay the licensing fees involved. While it would run like absolute crap, you could run all of this off of a single machine. It wouldn't be performant but then again you wouldn't really be doing anything with it to speak of. (That lack of performance is actually one of the drawbacks of the list. Ideally you'd have a couple of servers to spare.)

RHN Satellite, the Spacewalk "equivalent", for example, can cost thousands of dollars. Yeah, there's a developer license but -- if you know CentOS you know your way around RHEL, so there's really no point in having the newbie shell out more money than is necessary to achieve these ends.

I didn't recommend Ubuntu for the simple reason that you don't really see debian or Ubuntu in the "enterprise" Linux world outside of Amazon stuff. That's not to say it isn't ever used, just not in the kinds of shops I work at.

If I were to have a much more "exhaustive" list I'd push debian for the fact that it's more similar to other *NIXes; and I'd have an nginx and an httpd instance side-by-side for the web front-end. The JBoss Wiki is there specifically because Enterprise Linux administration means dealing with pain-in-the-ass java-based apps, and that's just how it is. <_<

#### Comment from other redditor
"Ideally you'd have a couple of servers to spare."
'Do I ever.'

#### Response from IConrad
In your case then I'd recommend that you scrap the "storage VM" and use an actual storage machine. Use FreeNAS and allocate your disks as a ZFS pool. The other machines would still be KVM, Xen, or ESXi hypervisors with iSCSI backing stores provided by the FreeNAS machine. Build your VMs accordingly. Make sure you name each CentOS OS instance's rootvg uniquely for that hostname, and then use zfs-autosnapshot on the backing machine in order to give you some "test to destruction" protection (if you screw up a machine you could just pull it's VM image out of an old snapshot). I'll leave as an exercise to the reader to figure out how to get kickstarts to name volume groups by hostname, and why that would be a good thing to do.


#### Comment from other redditor
"Thanks for this amazing post. A couple questions:
1. How exactly would you write a setup like this on your resume (say under a Projects/Homelab section)? That is, how could you write it succinctly enough but still convey the amount of tools/concepts used here?
2. Considering the amount of VMs running, what sort of system would have to be the host? I would think tons of memory...

#### Response from IConrad
Regarding point 2 -- check out KSM. Doesn't need much. These systems would be mostly idle so they wouldn't be doing much -- but otherwise they'd be pretty poorly performant overall regardless; I'm assuming this is for learning, not for using.

Regarding point 1) List "build and maintain home lab to test, upskill, and maintain enterprise-grade linux OS working environment, including many of the items listed in qualifications section." (Qualifications would include a list of technologies, bullet-point style, with a number showing years of experience in them. Flub this a little at first. "Approx. 1 year" yadda yadda.)

Bonus points if you include a .png/.jpg printout of a network architecture diagram (created via Visio / Dia) that shows your VM lab enviornment, as an additional attachment -- you could reference it. (This is bonus points especially since it demonstrates infrastructural documentation skills, which is something managers are always seeking.)

I've earned jobs in the past specifically because of the existence of my own home lab (which is a little more robust than this -- I've got a number of rack servers and a rackmountable switch at home.)


#### Excellent point made by another redditor
It's a good list. Let me just add a reminder to anyone who will try this to snapshot your vm's using virsh so that, when things go wrong (and they will), you can reset them.

The list is a little heavy on the applications and light on the actual operating system, so just go into it knowing that /u/IConrad is assuming you're going to be resilient and will fight through OS problems along the way. Sometimes OS issues can take longer than the actual exercise.

One OS issue that I think would be worth adding onto this list is that you really need to know the boot process / how to get around in grub and how UEFI/GPT works so that you can get to your data when things go wrong. This is an area that new people (especially in r/linux4noobs) totally destroy their own data while learning, it's good to practice alone at home -- you don't want your first exposure to this area to be at your new job.


#### Response from IConrad
Yeah, my goal was to provide practical exercises that would flesh out the skills an enterprise admin would need in order to handle the types of environments he (or she) might encounter. I intentionally left OS level breakfix out because I fully well expect someone building an environment with twenty something OSes each performing an infrastructural task they likely have never performed before to get stuff wrong and have to blow it up (intentionally or accidentally) multiple times. By no means would you come out of this an expert; but the research lessons needed to make them all work would teach them what internet sources to go to first for simple things like booting to the blinking cursor of doom.

#### Comment from other redditor
OT a bit: So if you're going Puppet {eventually} - why not Foreman + Katello vs Spacewalk? I'm versed in Foreman and Puppet, but our kickstart process is still boot CD image based, and updating that is one goal for our Scientific Linux 7 deployment.

Do you happen to know if Spacewalk would even be useful with Foreman managing PXE and Puppet managing configuration? Is Katello actually valuable here?

Also, Puppet Razor seems to be PE only and tech preview so . . . we're FLOSS, so again, not sure what that does that Foreman etc doesn't but if you have input, it'd be appreciated.


#### Response from IConrad
Katello is the successor to Spacewalk. I suggested what I suggested for the same reason I said to use CentOS 6 and not 7. Because it's more representative of enterprise environments. And because getting a working Spacewalk server running is simpler for someone with no prior expertise in Linux engineering. And because Spacewalk supports more distros than Katello does. And because being an enterprise admin means being able to handle legacy environments... Which is why I threw in the final element of making all of the previous work also compatible when CentOS 5. I almost included 4 as well.

There's absolutely no point in using Foreman in the walkthrough I listed. If you choose to do something else, it's on your head. You could certainly do it, but once you've got your head wrapped around Cobbler and you can re-engineer it for Razor, then doing it for Foreman would be no more of a challenge... And there's got to be a limit somewhere. I mean, you didn't see new reference any on the myriad other techs in existence, did you?
