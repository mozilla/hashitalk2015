# Good reads and tools we use
    
* 12factor.net : A great write up by Heroku on making an app scalable, modular, and easy to upgrade.  Basically, how to build apps in the new world.
* https://github.com/phrawzty/hiera-s3: Nice hiera plugin from phrawzty (but maybe just use vault! :) )
* Visible Ops Book: http://www.amazon.com/The-Visible-Ops-Handbook-Implementing/dp/0975568612
* Phoenix Project Book: http://www.amazon.com/The-Phoenix-Project-Helping-Business/dp/0988262592


# Code Examples We Use
* Our Terraform code: https://github.com/mozilla/socorro-infra/blob/master/terraform
* Our Puppet code: https://github.com/mozilla/socorro-infra/blob/master/puppet
* Our rustic deployment script: https://github.com/mozilla/socorro-infra/blob/master/bin/deploy-socorro.sh
* Our Packer code: https://github.com/mozilla/socorro-infra/blob/master/packer
* Our user data code: https://github.com/mozilla/socorro-infra/blob/master/terraform/webapp/socorro_role.sh


# Some of our example public dashboards / metrics 
* A cool way to cycle through dashboards: http://dashplay.io/0p98yofx+ 
* Our current events:  https://p.datadoghq.com/sb/282da3b8c-2916c1621c
* Today vs yesterday: https://p.datadoghq.com/sb/282da3b8c-3c0ed4ed8d
* Prod stats: https://p.datadoghq.com/sb/282da3b8c-bfee0403b5
* How to send custom and arbitrary metrics to cloudwatch (you can track # of coffees consumed, or anything else) https://www.loggly.com/blog/send-custom-metrics-to-cloudwatchs-api/

# Packer Hint
* Packer can create an AMI for any region, and even create an AMI for multiple regions.  Super useful for deployment!


# S3 Specific Hints
* US Standard is simply (US-East-1 and US-West-1) in failover mode.  This is why things in US Standard are 'eventual consistency' for reads after writes.  
* If you need immediate, reliable consistency, choose a specific region when creating the bucket.  
* US-Standard does not cost extra compared to single-region non-redundant S3 regions.
* Cloudfront costs the same per request as S3.
* You *do* get SSL with S3, but not on http endpoints.  ( Here, have this page explain it to you...it's a little weird.  http://docs.aws.amazon.com/general/latest/gr/rande.html )
* Otherwise, use SSL in cloudfront.  Use the old IAM tools to upload it...

# Other AWS Hints
* Our naming standards for AWS infrastructure(do this ASAP!): https://dl.dropboxusercontent.com/u/2273146/aws-naming-conventions.txt
* When given a choice, download/request your SSL certs in Apache format.  This goes right up to AWS easily.

**iam-servercertupload -b ./mysite.crt  -k ./mysite.key -c ./intermediatechain.crt -p /cloudfront/ -s mysite-2015-09-25**

* Always use autoscaling groups for everything, even things that never need to scale.  You always want 1 of some server up...autoscaling groups make it so if that one dies, a new one automatically replaces it.
* You can have multiple autoscaling groups scale into a single ELB.
* You can have a single autoscaling group add/remove servers from multiple ELBs.
* These concepts are useful if you want to balance out the # of instance store vs. EBS backed EC2 instances...EBS is usually the culprit on AWS @#$@storm days
* Go multi-region early rather than later...it doesn't get easier the longer you wait!
* When load testing or expecting heavy traffic, it's a great idea to call AWS support and have them "prewarm" your ELB.  ELBs are basically just EC2 instances running load balancing software, and 'prewarming' them is scaling them up.
* To get great ratings for SSL security, check out Mozilla's SSL cipher tool (credit to Gene!) https://mozilla.github.io/server-side-tls/ssl-config-generator/
* If you have to have multiple AZ's in staging but only have one node up, enable cross zone balancing.
* CNAME everything!  Let's say you have a database server:  webappdatabase.rdsaddress.2342342342.aws.com...  If you cname that to webapp-db.internaldomain.com instead, if you ever have to switch databases or regions you have a super easy way to switch over!
* Health check based routing is awesome, as is geo-ip routing.  
* Decide on a tagging strategy early on....Mozilla Tools and Services team uses:  environment, role, project.  These tags play a major role in monitoring, metrics, and even permissions.
* Always, immediately, first thing...enable Cloudtrail in AWS.

# Alerting Hint
* Setup an IFTTT recipe which will monitor the status RSS feeds for AWS, and SMS/email/alert you and your team when those RSS feeds get updated.
* Be sure you have multiple sources watching your apps!

# Load Testing Hint
* Easy 'pound one address' tool:  bees with machineguns  (it's awesome to make an endpoint that exercises the entire app/funnel/workflow that you can then pound on with bees...and the logs are so fun to read)



# Ways to Not Spend All of Your Organizations Money 
* Setup detailed billing and a bucket to store it im
* Try out Netflix ICE cost analysis software
* When you know you need an instance for at least a year, setup reserved instances

# Team Hint
* A Great Intro to 'Gamedays' (The Way to Make Apps and Teams More Resilient by Dylan Richard): https://www.youtube.com/watch?v=LCZT_Q3z520  

# A hint from @rhelmer
ABCD:  ALWAYS BE CHECKING DATADOG

