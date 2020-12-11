# Hygieia-simple
Hygieia搭建步骤
1.	了解hygieia基础 信息
	Hygieia.DOCX

2.	本地安装Hygieia开发用到技术栈
Hygieia主要java开发的，使用了spring boot框架，前端使用angular.js开发

前端相关：node、npm、
bower（npm install -g bower）、gulp（npm install -g gulp）
后端相关：java、spring boot、maven
数据库：mongoDB
其中数所有的据都是存储在mongoDB中

3.	从github上clone hygieia基础项目
https://github.com/wanggerui/Hygieia-simple.git
4.	启动MongoDB
	在Mongodb的bin目录下 cmd 进行创建用户（UI同账号密码）

mongo

use dashboarddb

db.createUser({user: "dashboarduser",pwd: "dbpassword", roles: [ {role: "readWrite", db: "dashboarddb"}]})

5.	启动API	 (默认端口：8080:不需要修改)
1)	查看Hygieia/api 的application.properties
修改本地mongodb数据库信息及其余基础配置
2)	加载依赖    mvn install
3)	启动项目
Application或者：
Java -jar  I:\Hygieia\Hygieia6\Hygieia\api\target\api.jar  
--spring.config.location=I:\Hygieia\Hygieia6\Hygieia\api\src\main\resources\application.properties

注：修改红色部分目录
测试： http://localhost:8080/api/ping  为true

以下为api的生成口令方式：（可略过，直接启动在UI页面打印的控制台获取）
java -jar I:\Hygieia\Hygieia6\Hygieia\api\target\api.jar --spring.config.location=I:\Hygieia\Hygieia6\Hygieia\api\src\main\resources\applicatio-Djasypt.encryptor.password=dbpassword

6.	启动UI 	(默认端口：3000:不需要修改)
1)	加载依赖   npm install ，bower install
2)	启动项目   gulp serve
测试：启动后跳转3000页面可登录注册

7.	启动collector
以scm插件gitlib为例：
1)	在resources 下面修改 application.properties
修改名称，端口 
github的token（网上查找生成口令）
2)	启动：
java -jar 
I:\Hygieia\Hygieia6\Hygieia\hygieia-scm-github-graphql-collector\target\github-graphql-scm-collector.jar --spring.config.location=I:\Hygieia\Hygieia6\Hygieia\hygieia-scm-github-graphql-collector\src\main\resources\application.properties
8.	配置UI
1.1	Scm仓库--	git
application.properties:
#Database Name
dbname=dashboarddb

#Database HostName - default is localhost
dbhost=localhost

#Database Port - default is 27017
dbport=27017

server.port=8099

#Database Username - default is blank
#dbusername=db

#Database Password - default is blank

#dbpassword=dbpwd

#Collector schedule (required) 每10分钟执行任务一次
github.cron=0 */10 * * * *

github.host=github.com

#Maximum number of days to go back in time when fetching commits
github.commitThresholdDays=60

#Optional: Error threshold count after which collector stops collecting for a collector item. Default is 2.
github.errorThreshold=10
#This is the key generated using the Encryption class in core
github.key=5fb4b6b***************

#personal access token generated from github and used for making authentiated calls

#github.personalAccessToken=
github.personalAccessToken=a90db1*******************************





UI:
 


1.2	Jira
 
 
 
 
application.properties：
#Collector schedule (required)
dbname=dashboarddb
#dbhost=localhost
#dbport=27017

feature.cron=*/30 * * * * ?
#feature.cron=0 */5 * * * *


#Page size for data calls (Jira maxes at 1000)
feature.pageSize=100

#In-built folder housing prepared REST queries (required)
feature.queryFolder=jiraapi-queries

#Jira API Query file names (String template requires the files to have .st extension) (required)
feature.storyQuery=story
feature.epicQuery=epic

feature.projectQuery=projectinfo
feature.memberQuery=memberinfo
feature.sprintQuery=sprintinfo
feature.teamQuery=teaminfo
feature.trendingQuery=trendinginfo



#Trending Query:  Length of sprint week (not-required)
#feature.sprintEndPrior=2

#Scheduled Job prior minutes to recover data created during execution time (usually, 2 minutes is enough)
feature.scheduledPriorMin=2

#Delta change date that modulates the collector item task - should be about as far back as possible, in ISO format (required)
feature.deltaCollectorItemStartDate=2018-01-01T00:00:00.000000

#Jira Connection Details
feature.jiraBaseUrl=http://10.45.*.*:8080
feature.jiraQueryEndpoint=rest/api/2/

#64-bit encoded credentials with the pattern username:password

#on a mac you con create them with : echo "username:password" | base64
#reference:  https://www.base64decode.org/


feature.jiraCredentials=MT*****************************Iz


#Start dates from which to begin collector data, if no other data is present - usually, a month back is appropriate (required)
feature.deltaStartDate=2019-02-01T00:00:00.000000
feature.masterStartDate=2019-02-01T00:00:00.000000

#In Jira, general IssueType IDs are associated to various "issue"
#attributes. However, there is one attribute which this collector's
#queries rely on that change between different instantiations of Jira.
#Please provide a String Name reference to your instance's IssueType for
#the lowest level of Issues (e.g., "user story") specific to your Jira
#instance.  Note:  You can retrieve your instance's IssueType Name
#listings via the following URI:  https://[your-jira-domain-name]/rest/api/2/issuetype/
#Multiple comma-separated values can be specified.
#feature.jiraIssueTypeNames=Story,Epic,Task,Sub-task
feature.jiraIssueTypeNames=故事,缺陷,任务,子任务,Epic



#In Jira, your instance will have its own custom field created for "sprint" or "timebox" details,
#which includes a list of information.  This field allows you to specify that data field for your
#instance of Jira. Note: You can retrieve your instance's sprint data field name
#via the following URI, and look for a package name com.atlassian.greenhopper.service.sprint.Sprint;
#your custom field name describes the values in this field:
#https://[your-jira-domain-name]/rest/api/2/issue/[some-issue-name]
feature.jiraSprintDataFieldName=customfield_10001


#In Jira, your instance will have its own custom field created for "super story" or "epic" back-end ID,
#which includes a list of information.  This field allows you to specify that data field for your instance
#of Jira.  Note:  You can retrieve your instance's epic ID field name via the following URI where your
#queried user story issue has a super issue (e.g., epic) tied to it; your custom field name describes the
#epic value you expect to see, and is the only field that does this for a given issue:
#https://[your-jira-domain-name]/rest/api/2/issue/[some-issue-name]
feature.jiraEpicIdFieldName=customfield_11800
#feature.jiraEpicIdFieldName=customfield_10008
#customfield_11800

#In Jira, your instance will have its own custom field created for "story points"
#This field allows you to specify that data field for your instance
#of Jira.  Note:  You can retrieve your instance's storypoints ID field name via the following URI where your
#queried user story issue has story points set on it; your custom field name describes the
#story points value you expect to see:
#https://[your-jira-domain-name]/rest/api/2/issue/[some-issue-name]
feature.jiraStoryPointsFieldName=customfield_10006



#In Jira, your instance will have its own custom field created for "team"
#This field allows you to specify that data field for your instance
#of Jira.  Note:  You can retrieve your instance's team ID field name via the following URI where your
#queried user story issue has team set on it; your custom field name describes the
#team value you expect to see:
#https://[your-jira-domain-name]/rest/api/2/issue/[some-issue-name]
feature.jiraTeamFieldName=

#Set this to true if you use boards as team
#feature.jiraBoardAsTeam=false
feature.jiraBoardAsTeam=true



#Story,Bug,Task,Sub-Task,Epic
feature.jiraStoryIds[0]=11739
feature.jiraStoryIds[1]=11900
feature.jiraStoryIds[2]=10106
feature.jiraStoryIds[3]=10102
feature.jiraEpicId=11800

1.3	Jenkins
 
 
 
 

application.properties:
#Database Name 
dbname=dashboarddb 
 
#jenkins.servers[0]=http://localhost:8181/jenkins/
jenkins.servers[0]=http://10.45.136.151:9999/
jenkins.usernames[0]=yay
jenkins.apiKeys[0]=116ed4863f449d202f58e4fa290171a2b3
#上面是API Tokens，在Jenkins用户里面可以配置
#Collector schedule (required)
jenkins.cron=0 0/5 * * * * 

jenkins.saveLog=true

启动：
java -jar D:\softwareback\devops\hygieia\Hygieia-3.0.2\collectors\build\jenkins\target\jenkins-build-collector-3.0.1.jar --spring.config.location=D:\softwareback\devops\hygieia\Hygieia-3.0.2\collectors\build\jenkins\application.properties

1.4	Scm仓库--	svn
 
application.properties:
#Database Name
dbname=dashboarddb

#Database HostName - default is localhost
dbhost=localhost

#Database Port - default is 27017
dbport=27017


#Logging File location
#logging.file=./logs/subversion.log

#Collector schedule (required)
subversion.cron=*/30 * * * * ?
#subversion.cron=0 0/5 * * * *
#subversion.cron=0 */15 * * * *

#注意下面这种地址不要配置，用户和口令也不要配置
#subversion.host=https://10.45.136.230/svn/ZYN/HGARCH/branch/V1.0/code/personrecord

#Maximum number of previous days from current date, when fetching commits
subversion.commitThresholdDays=30

启动：
java -jar D:\softwareback\devops\hygieia\Hygieia-3.0.2\collectors\scm\subversion\target\subversion-collector-3.0.1.jar --spring.config.name=subversion  --spring.config.location=D:\softwareback\devops\hygieia\Hygieia-3.0.2\collectors\scm\subversion\target\classes\application.properties

1.5 sonar
#Database Name

dbname=dashboard

#Database HostName - default is localhost

dbhost=localhost

#Database Port - default is 27017

dbport=27017

#Database Username - default is blank

#dbusername=db

#Database Password - default is blank

#dbpassword=dbpwd

#Collector schedule (required)

sonar.cron=0 */1 * * * *

sonar.servers[0]=http://localhost:9000

#Sonar Authentication Username - default is blank

sonar.usernames[0]=username

#Sonar Authentication Password - default is blank

sonar.passwords[0]=pwd

#Sonar Metrics

sonar.metrics[0]=ncloc,line_coverage,violations,critical_violations,major_violations,blocker_violations,sqale_index,test_success_density,test_failures,test_errors,tests

#Sonar Version - see above for semantics between version/metrics
#sonar.versions[0]=
#sonar.tokens[0]=1111111111111


