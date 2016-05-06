#### Upgrade
surum: 1.6
http://www.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz
mv /home/dpic/apps/apache-flume-1.6.0-bin/conf/flume-conf.properties{,.org}
cp /home/logger/flume/conf/flume-conf.properties /home/dpic/apps/apache-flume-1.6.0-bin/conf/
mv /home/dpic/apps/apache-flume-1.6.0-bin/conf/flume-env.sh.template{,.org}
cp /home/logger/flume/conf/flume-env.sh.template /home/dpic/apps/apache-flume-1.6.0-bin/conf/
chown dpic: /home/dpic/apps/apache-flume-1.6.0-bin -R





