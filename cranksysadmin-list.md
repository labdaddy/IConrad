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
