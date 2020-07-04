#### To Install Foreman
Foremam requires puppet to be installed on the system first.
To install puppet: `sudo yum -y install https://yum.puppet.com/puppet6-release-el-7.noarch.rpm`
Next, the system should have Katello installed as well.
This is not made explicitly clear in the Foreman docs, in fact it is presented as optional. 
Based on the contents of a youtube screenshare of
The Foreman setup process it appears that Katello
Is actually required. So we are installing Katello as well.

