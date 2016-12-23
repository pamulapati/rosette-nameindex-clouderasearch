To get started, you will need a CDH cluster with Cloudera Search/Solr services running on Java SDK 1.8 or later. 

Then, install Rosette on all the Solr nodes of the CDH cluster. To do this, you’ll need to unzip the SDK and Documentation files to the same directory (which we will call BT_ROOT) and then copy the license file to the BT_ROOT/rlp/rlp/licenses subdirectory.

You must include a Java property setting that points to the root of a Rosette SDK,. You can achieve this in Cloudera Manager, by searching “Java Configuration Options for Solr Server”  and appending the existing value with  -Dbt.root=<BT_ROOT> (e.g. -Dbt.root=/usr/bt/rlp). Click “Save Changes” and restart your Solr services.

<screenshot>

You can use the project made available in this git to get you started  by pulling the namesearch folder to /root/namesearch on a node in the cluster

<screenshot>
export PROJECT_HOME=/root/namesearch


vi conf/schema.xml and observe
<fieldType name="bt_rni_name" class="com.basistech.rni.solr.NameField"/>


<field name="primaryName" type="bt_rni_name" indexed="true" stored="true"/>


vi conf/solrconfig.xml and verify that the [[BT_ROOT]] matches to where you unzipped Rosette   SDK

<lib path="[[BT_ROOT]]/lib/jvm/bt-adm-model-2.1.2.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/btcommon-api-36.0.2.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/btcommon-api-jackson-36.0.2.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/btcommon-lib-37.0.1.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/btrlpnc.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/bt-rni-solr[[xxx]]-plugins.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/bt-rni-solrj[[xxx]]--client.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/icu4j-55.1.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/log4j-1.2.15.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/slf4j-api-1.6.4.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/slf4j-jdk14-1.6.4.jar"/>
<lib path="[[BT_ROOT]]/lib/jvm/slf4j-log4j12-1.6.4.jar"/>




Create namesearch insteance directory in SolrCloud
sudo solrctl --zk solrtest-1.vpc.cloudera.com:2181,solrtest-2.vpc.cloudera.com:2181,solrtest-3.vpc.cloudera.com:2181/solr instancedir --create namesearch $PROJECT_HOME

Create namesearch collection in SolrCloud
sudo solrctl --zk solrtest-1.vpc.cloudera.com:2181,solrtest-2.vpc.cloudera.com:2181,solrtest-3.vpc.cloudera.com:2181/solr collection --create namesearch -s 3 -r 1

You have name search application ready.




