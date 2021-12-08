## Digital Ocean K8 Challenge

Repo for the Digital Ocean K8s Challenge

## Challenge

Deploy a Log Monitoring stack. Decided to go with ELK (fairly standard), builidng in Filebeats and Logstash for internal cluster monitoring.

Kibana front-end can be viewed [here](https://159.65.208.143/) (a password is necessary, contact me if you want to view).

## Solution

I decided to undertake this challenge using only Manifests, apart from the deployment of the ECK Operator, for which I used a Helm repo.
Initially started with the deployment of Elasticsearch and Kibana, to ensure a base for the logging stack. The manifest for both are in the file elastic_kibana.yaml.

One thing to note that was a challenge was managing resources. I found myself having to resize my pods regularly as I wanted to deploy more services. Perhaps this is due to my lack of forsight, understanding of node resourcing, or something left to be learned in my way-of-working with Kubernetes.

## Notable Debugging 

During the deployment of Logstash, I found myself spending a lot of time reconfiguring the Logstash Config-map, which contained the Grok filter for filtering logs coming into the pipeline. I tried many different Grok interpreters online but in the end settled on no filter, as I wasn't focusing on any specific logs. Though I do understand the benefit of filters, and can see how they may be applied to aggregated logging across a large microservice architecture using many different technologies.

Interestingly, Kibana dashboards were a hugely beneficial tool in finding and monitoring this issue. I developed a dashboard tracking the count of logs being indexed from Logstash (`index: logstash-*`) in a line graph, and matched against the count of logs being ingested with the tag `_grokfailure`. This allowed me to follow the impact of the changes I was making to the stack. As I re-configured and re-deployed Logstash and Logstash-configmap I could track that logs were in fact being indexed, and how many of them were attributable to a Grok filtering failure!

![al text][grok failure plotting]

[grok failure plotting]: https://github.com/harrywm/do-k8-challenge/blob/master/resources/grokfailure.png
