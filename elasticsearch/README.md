Elastic search Runbook

# Infrastructure setup
![alt text](https://github.com/AutoScout24/ghostbuster_runbooks/blob/master/elasticsearch/elastic_infra.png "Elasticsearch Infrastructure")
 
#### Elasticsearch clusters
Every cluster contains [currently] 10 machines, running Elasticsearch 1.7.5

Cluster 1:
- LELAVMV001
- LELAVMV003
- ...
- LELAVMV019

Cluster 2:
- LELAVMV002
- LELAVMV004
- ...
- LELAVMV020
 
Every cluster has a build server which is used for new index creation in background mode
Cluster 1: LELAVMV001
Cluster 2: LELAVMV006

#### Varnish machines for caching
LPRXVAV001 - LPRXVAV004

#### WebAPI
LWEBEWV001-LWEBEWV012

# Checking partial update
There is a TC agent (lelabuv001) running cron job for updating ES cluster with newest data (every 2 min period)

#### Via TC agent (deep level):
Login into lelabuv001 (ssh or putty e.g. via lteropv002.as24.local)

sudo su - deploy

crontab -l

Shows existing 2 cron jobs (LIVE1 & LIVE2) , you will something like this:

*/2 * * * * cd /opt/buildagent/scripts/LIVE2-classifieds-46/IndexSetup; source ./activate.sh; python ./partialBuild.py environment_name=LIVE2 build_number=46 >> /opt/buildagent/scripts/LIVE2-classifieds-46/log_symlink/cron.log 2>> /opt/buildagent/scripts/LIVE2-classifieds-46/log_symlink/cron.err # LIVE2_partial_classifieds
 
Take a look into logfile /opt/buildagent/scripts/LIVE2-classifieds-46/log_symlink/cron.log for e.g. last 200 lines
tail -200 /opt/buildagent/scripts/LIVE2-classifieds-46/log_symlink/cron.log
 
#### Via Icinga (high level):
Login https://icinga2.admin.autoscout24.com

Look for LWEBEWV, click on a machine, click to Service tab

information regarding last partial update is displayed

 
# Checking cluster status
#### Per URL
Login in the live zone (e.g. lteropv002.as24.local)

curl or browser with following adress: LELAVMV002:9200/_cluster/health?pretty
 
#### Inciga
Login https://icinga2.admin.autoscout24.com

Look for LWEBEWV, click on a machine, click to "Service" tab

information regarding status is displayed

# Team City Builds
https://teamcity.as24.local/project.html?projectId=VehicleMarketElasticsearch&tab=projectOverview
 
# ES support
(https://support.elastic.co/)

List of Ppeople who can create tickets:
* Simon Mittermueller        
* Inaki Anduaga        
* Marija Bajic        
* Radu Crican        
* Juri Smarschevski
* Philipp Garbe        
* Diego Toharia        
