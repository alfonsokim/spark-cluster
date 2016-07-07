
FROM alfonsokim/ubuntu_java
MAINTAINER alfonso.kim

# =======================================================
ENV DEBIAN_FRONTEND noninteractive

USER root

# -------------------------------------------------------
RUN apt-get update -y
RUN apt-get upgrade -y

RUN apt-get -y install curl

# -------------------------------------------------------
# Scala
RUN curl -s wget http://www.scala-lang.org/files/archive/scala-2.11.5.tgz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./scala-2.11.5 scala
ENV SCALA_HOME=/usr/local/scala
ENV PATH=$PATH:$SCALA_HOME/bin

# SBT
RUN wget http://apt.typesafe.com/repo-deb-build-0002.deb
RUN dpkg -i repo-deb-build-0002.deb
RUN apt-get -y update
RUN apt-get -y install sbt
RUN rm repo-deb-build-0002.deb

# -------------------------------------------------------
# hadoop
RUN curl -s http://www-us.apache.org/dist/hadoop/common/hadoop-2.7.2/hadoop-2.7.2.tar.gz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./hadoop-2.7.2 hadoop

ENV HADOOP_PREFIX /usr/local/hadoop
ENV HADOOP_COMMON_HOME /usr/local/hadoop
ENV HADOOP_HDFS_HOME /usr/local/hadoop
ENV HADOOP_MAPRED_HOME /usr/local/hadoop
ENV HADOOP_YARN_HOME /usr/local/hadoop
ENV HADOOP_CONF_DIR $HADOOP_PREFIX/etc/hadoop
ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop

# RUN sed -i '/^export JAVA_HOME/ s:.*:export JAVA_HOME=/usr/java/default\nexport HADOOP_PREFIX=/usr/local/hadoop\nexport HADOOP_HOME=/usr/local/hadoop\n:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN sed -i '/^export JAVA_HOME/ s:.*:export JAVA_HOME=/usr/lib/jvm/java-8-oracle\nexport HADOOP_PREFIX=/usr/local/hadoop\nexport HADOOP_HOME=/usr/local/hadoop\n:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN sed -i '/^export HADOOP_CONF_DIR/ s:.*:export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop/:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh

RUN chmod +x $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN chmod +x $HADOOP_PREFIX/sbin/*

RUN mkdir $HADOOP_PREFIX/input
RUN cp $HADOOP_PREFIX/etc/hadoop/*.xml $HADOOP_PREFIX/input

# -------------------------------------------------------
# Spark
RUN curl -s http://d3kbcqa49mib13.cloudfront.net/spark-1.6.1-bin-hadoop2.6.tgz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./spark-1.6.1-bin-hadoop2.6 spark

RUN cp /usr/local/spark/conf/slaves.template /usr/local/spark/conf/slaves

# -------------------------------------------------------
# Ambiente
RUN /bin/cp -f /usr/share/zoneinfo/America/Mexico_City /etc/localtime

# -------------------------------------------------------
# Inicio

# Spark ports
EXPOSE 4040 8080
# Hdfs ports
EXPOSE 9000 50010 50020 50070 50075 50090
# Mapred ports
EXPOSE 19888
#Yarn ports
EXPOSE 8030 8031 8032 8033 8040 8042 8088
#Other ports
EXPOSE 49707 2122

CMD ["/bin/bash"]
