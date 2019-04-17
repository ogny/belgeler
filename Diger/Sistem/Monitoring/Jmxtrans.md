Jmxtrans is a tool which allows you to connect to any number of Java Virtual
Machines (JVMs) and query them for their attributes 

If you use the SpringFramework for your code, it can be as easy as just adding
a couple of annotations to a Java class file.

The query language is based on the easy to write JSON format. 
The results of the queries are processed by Java classes called OutputWriters.
(integrating Java code with a third party tool such as Graphite)

### Engine Mode
This engine will read a directory of .json files, process them and then create
'jobs' to execute on a cron-like schedule. 

##### H5

* [jmxtrans wiki](https://github.com/jmxtrans/jmxtrans/wiki)
