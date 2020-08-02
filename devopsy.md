##### Nothing teaches like doing. Try your hand at building these three things and put the results in your github account. I regularly interview devops candidates, if a candidate sent me these things I would 100% schedule an interview.

Pick a config management system like chef, puppet, salt, or ansible, and code up a system that will take a fresh linux install and deploy your web app to it so it's ready to serve requests. Bonus points for wiring it up to Packer so you can build AMI's (AWS EC2 instance machine images).

Write a bunch of terraform to create all the moving parts needed to host your app in a robust way. A VPC, subnets in multiple availability zones, autoscaling groups, load balancer (don't forget health checks), datastores, etc. (Note: as long as you don't leave this stuff running and just stand it up while you're hacking it can be done quite cheaply, like 5-10 bucks / month). Edit: I guess you could do this in Cloudformation too. I am personally biased towards Terraform though but either one would demonstrate you understand how to manipulate cloud resources using these tools.

Pick a CI tool, like running Jenkins in a local VM, or CircleCI or whatever and create a build and deploy process wired up to the github repo for your web app. When a PR is cut CI runs tests and sends results to github. When you merge to master CI deploys the latest version of your app to the hosting environment you've built.

Bonus points: containerize your app and try running it in ECS or GKE (or EKS whenever that is available).
