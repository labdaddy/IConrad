Get started with linux just enough to be useful
Discussion
I see people on here trying to learn Linux, but I feel like a lot of them take the wrong path and either try to learn Linux using a cert of some kind, or try to learn it on their own but focus on the wrong stuff.

You don't actually have to be an expert, or learn the entire platform from top to bottom. There are ways you can learn things that make you immediately useful in a mixed environment with a decent Linux footprint.

First, the stuff you shouldn't waste time on in my opinion (you can always return to this stuff later):

• Desktop linux. In reality you're going to be managing linux boxes via SSH from a Mac or Windows machine. If you have a spare PC and want to set it up there's nothing wrong with that, but it's only marginally useful career-wise to get an Ubuntu desktop going and get web browsers and stuff going. You're probably not going to be managing Linux desktops.

• Focusing overly on Samba as a replacement for Windows infrastructure. The reality is even in heavily Linux corporate environments (we're like 70% Linux right now) we still use Microsoft AD and Windows for file servers. This just isn't what most enterprise environments use Linux for. Microsoft excels in this area and nothing competes with AD. Putting brain cycles into that doesn't make sense.

• Linux as a virtualization platform seems to be where a lot of the new-to-linux people want to go, but again this is kind of a waste of time. The reality is, you're going to be running linux on top of vSphere, AWS or Hyper-V most of the time. So just do that. You don't have to learn everything.

• There's an overly complex "how to learn linux" guide that r/sysadmin loves (and I hate) because it focuses way too much on the stuff I'm telling you doesn't matter as much if you just want to be functional, and it does it in a weird order.

Instead of all that, focus on stuff that can give you an immediate career impact.

• Understand managing users and groups. Understand how this differs from Windows and the pros and cons. Understand permissions as well, and again how this differs from Windows.

• Understand services and how to start and stop them, how to tell if something is running, how to set something to start when the machines boots, etc. Know how to look at running processes and kill them if necessary. Be able to tell when a machine is performing poorly.

• Understand file operations. Know how to create and delete files and directories. Know how to search through text files and search for a particular string. Know how to use vim and don't cheat with pico or nano.

• Understand networking well enough to configure a static IP address and do some troubleshooting. Understand iptables or firewalls enough you can make the changes you need to the local firewall.

• Know how to install and remove packages using yum or apt.

• Learn the LAMP stack. Be able to install php, mysql and apache and know how to troubleshoot each of them. Be able to make a basic hello world application in PHP. Know some basic SQL so you can dump a database on one machine and import it on another. You don't have to know everything about SQL. Know how to do basic queries and look at tables.

• Understand where logs are located and how to look at them.

• Figure out how to do some basic automation. If you have minimal bash skills as mentioned above you can write a shell script. It's that easy. Maybe throw some ansible on top of that since it's the easiest config management tool to do really basic stuff with.

• Learn about monitoring. Nagios is a good place to start even though everyone hates it.

The goal with everything I'm saying here is to become a contributor to an existing team and be able to do Linux work. This isn't how you become a senior linux architect, but the goal is to just be functional and you can learn more later.

The problem is too many people try to learn linux from the ground up, see it as too complex, get distracted by the stuff I mentioned early on that has less immediate usefulness in their career, and never really get anywhere with it.

A Windows admin who understands the basics of troubleshooting of a LAMP environment and can look at logs and edit config files is infinitely more useful than the guy who has an Ubuntu desktop he's trying to watch movies on and has been fucking around with virtualization and samba. I don't understand why so many early Linux users get so fixated on desktop usage, samba and virtualization when these 3 things don't matter as much as the stuff I mentioned.

COMMENT
"Desktop linux. In reality you're going to be managing linux boxes via SSH from a Mac or Windows machine"

-Implying its not the year of the linux desktop...

heh, On a more serious note. I completely agree. I'm only a few years into my first IT job, a junior linux admin position on a team responsible for the os and infra layer for 3k+ rhel servers. In that time nearly everything you mentioned has come up consistently. The one thing I might add is learning the basics for how the OS can be installed: pxeboot, deploy from images, kickstarts, old fashioned install from disk. etc.

The way it should be looked at is that the OS exists to support services and the services exist to support the company. You should focus on things that affect the system's ability to run services.

Is this running?

how do I stop/start/restart it?

Where does it write its errors to?

Does it have permissions to write its errors/results/output/etc?

Are things able to connect to your service? networking/firewall.

Who needs to manage the service and do they need root access to do so?

Can bad guys access your service?

Is there a enough disk space for the service's data?

If that disk dies what happens to your service?

How do you preserve that data?

Will you notice if that service stops working as expected?

If you need another system to handle services how do you get it setup?

COMMENT
Just a word of warning, if you're going to mess around with pxe, just skip plain pxe immediately and go straight to ipxe. Trust me, it will preserve your sanity immensely. TFTP can go die in a fire.

COMMENT
Only thing I would add is:

how do i reconfigure the service, either with a ".conf" file or with its own reconfigurator thingamadoodle.

COMMENT
Is there a enough disk space for the service's data?

Even if there is enough space. Is there enough space in /tmp? Lots of things write to temp (webservers, mail clients, auth cough kerberos cough).

Developers love to put their files on /tmp, which inevitably fills up temp. Which then takes the webserver/mailserver/login prompt down.

COMMENT
"There's an overly complex "how to learn linux" guide that r/sysadmin loves (and I hate) because it focuses way too much on the staff I'm telling you doesn't matter as much if you just want to be functional, and it does it in a weird order."

Hah! (IConrad responds)

It's funny that you say that. I wrote that list in order to give the "supplicant" an opportunity to dash themselves upon the rocks of the art. I have long believed that self-learners need goals, things to identify as hallmarks in the process.

From there, it's a question of what order those things should exist in. I wrote, largely, in terms of dependencies; what needs to exist first in order to allow the next thing to exist.

I very specifically did not call out one text editor over another; nor did I suggest one hypervisor solution over another really -- yeah IIRC I suggested KVM but that was just to keep it all "in-house". Each "step" had a reason for being suggested; and the reason for the entire collection of steps was to represent a first-time exposure to conceptual categories of things that the daily work of the Linux/UNIX sysadmin might entail. Amongst other things you pick up knowledge of:

Managing virtualized OS instances

Server provisioning / automation

Server configuration management

Patch management in clustered environments

Load balancers

Network architecture

User management

HA Architecture

Web-app stack architecture

Researching and implementing solutions on the basis of partial specifications.

Etc..

These are, frankly put, the skills that are essentially core competencies of the *NIX sysadmin.

I have worked at a goodly number of organizations over the years and I can say that if you don't understand how to handle older Linux distributions, you're not going to be as effective as you could otherwise be. Similarly; you can't successfully integrate an LDAP schema into your *NIX systems without having a solid understanding of how GID/UID and group hierarchies work in the PAM world. Just isn't going to happen.

And as to specifically knowing LAMP -- I've gone roughly ten years into this career with barely any more knowledge of LAMP specifically than what I had starting out; it was hardly an obstacle considering I have extensive experience with alternatives in that solution-space and the rest is just a couple of google-searches away (which is a skill you're not going to complete my "walkthrough" without picking up. This is by design.)

Where your suggestions and mine differ most, from what I can tell, are here: what you describe for a skillset is a great beginning place for a senior operator or a very junior sysadmin, whereas I was intending to provide a vehicle for someone to pick up the core competences necessary to be able to know how to learn about whatever their sysadmin career might throw at them.

The exact hypervisor you learn about is orders of magnitude less relevant than having the basic knowledge about hypervisors, etc., etc..



level 2
admiralspark
Cat Tube Secure-er
6 points
·
2 years ago
Hey, want to thank you for that original writeup. I've used it for years now to get friends and coworkers deeper into enterprise linux, and it's been invaluable for that. Had it printed and put on my wall for when people ask me "how did you learn all of this linux stuff?".

I specifically remember how it included a JBoss-based application which got people into working with middleware and enterprise apps, which was huge. So if you want to write a slightly modernized version of it, I'd love to read it :)

Having read a bunch of cranky's posts before, I think his enterprise environment just isn't the same as an F-50 or a publicly traded company, and the needs there aren't the same as needs elsewhere.



level 2
a_wild_thing
3 points
·
2 years ago
Haha the gangs all here. I started that thread under one of my alts. Thanks again for your response, sounds like it helped a lot of people.

In the end I never got to make that transition to Linux Admin, which I'll probably always regret, however I ended up going into Pre Sales / Solution Architecture so alls well that ends well.



level 2
[deleted]
1 point
·
2 years ago
I still point people to that guide as a way of showing them the scope of what businesses will use Linux for.



level 2
[deleted]
1 point
·
2 years ago
Exactly, I showed it to someone who is a systems architect working mainly in cloud-based virtualization for large clients... He said it was a little dated, but totally what he works with all day.

It's weird that cranky thought it wasn't relevant. Maybe he can't see past his own landscape?
