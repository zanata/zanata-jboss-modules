Zanata JBoss Modules
====================

This project packages modules for Zanata which can be used on JBoss EAP 6.
It currently includes some modules which together enable Logstash/JSON
formatted log files.


Usage
-----

Setting up JSON logging:
1. Extract zanata-jboss-modules-$version.zip to your jboss-eap directory.
2. Configure jboss to use Logstash format by editing the following into
   your standalone(-full).xml:

        <!-- This should replace the previous FILE handler -->
        <periodic-rotating-file-handler name="FILE" autoflush="true">
            <formatter>
                <named-formatter name="logstash"/>
            </formatter>
            <file relative-to="jboss.server.log.dir" path="server.log.json"/>
            <suffix value=".yyyy-MM-dd"/>
            <append value="true"/>
        </periodic-rotating-file-handler>

        <!-- This should appear after <root-logger>...</root-logger>  -->
        <formatter name="logstash">
            <custom-formatter module="org.jboss.logmanager.ext" class="org.jboss.logmanager.ext.formatters.LogstashFormatter"/>
        </formatter>


To run Logstash (and Elasticsearch) against your logs:
1. Download and extract version 1.5.4: https://www.elastic.co/downloads/logstash
2. copy in [logstash.conf](etc/logstash.conf)
3. edit the path to your json log file
4. run logstash: bin/logstash agent --verbose -f logstash.conf --debug


Finally, to run Kibana against Elasticsearch:
1. Download and extract kibana 4.1.1: https://www.elastic.co/downloads/kibana
2. Run bin/kibana
3. Launch http://0.0.0.0:5601/


Links
-----
* [Centralized Logging for WildFly with the ELK Stack](http://wildfly.org/news/2015/07/25/Wildfly-And-ELK/)
* [jboss-logmanager-ext](https://github.com/jamezp/jboss-logmanager-ext)
* [JSON Processing](https://jsonp.java.net/)


Licence
-------
* Build scripts are under the [Mozilla Public License, Version 1.1](http://www.mozilla.org/MPL/1.1/)
* jboss-logmanager-ext is under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
* JSON Processing (JSR 353 Reference Implementation) is under [CDDL+GPLv2 with classpath exception](https://jsonp.java.net/license.html)


Authors
-------
Sean Flanigan <sflaniga@redhat.com>


Acknowledgements
----------------

Module build scripts are based on [dcm4chee-arc-cdi's json-jboss-modules](https://github.com/dcm4che/dcm4chee-arc-cdi/tree/4.4.0.Beta1/json-jboss-modules) by [Hesham Elbadawi](https://github.com/Hesham-Elbadawi).
