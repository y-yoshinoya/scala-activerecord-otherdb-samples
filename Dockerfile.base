FROM ubuntu:14.10
RUN apt-get update && apt-get -y upgrade
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y install openjdk-8-jdk git
RUN mkdir /opt/apps
RUN cd /opt/apps && \
   git clone https://github.com/aselab/scala-activerecord-sample && \
   cd scala-activerecord-sample && \
   bin/sbt compile
RUN echo '\
libraryDependencies ++= Seq(\n\
  "mysql" % "mysql-connector-java" % "5.1.34", \n\
  "org.postgresql" % "postgresql" % "9.4-1200-jdbc41", \n\
  "com.oracle" % "ojdbc6" % "11.2.0.3"\n\
) \n\n\
resolvers += "maven" at "https://code.lds.org/nexus/content/groups/main-repo"\
' > /opt/apps/scala-activerecord-sample/play2x/db-dpendencies.sbt
RUN cd /opt/apps/scala-activerecord-sample && bin/sbt compile
