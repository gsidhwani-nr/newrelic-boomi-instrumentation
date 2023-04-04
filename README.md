# newrelic-boomi-instrumentation
This repository is only carrying the instrumenation binaries for Boomi runtime 

# What are the componets where new relic agent is required to be deployed

The '**Atom Node**' JVM which handles the general management and housekeeping for the node.  Not responsible for any actual data integration processing.  This JVM does currently have a New Relic Agent associated with it.
Currently the Atom Cloud telemetry data that is provided to the New Relic platform comes from the 'Atom Node' JVM.  This is limited to higher level information about the Atom Cloud (e.g., number of processes running, number of attached sub-accounts, average execution time, queued executions, cluster issues).
This is essentially the monitoring that our own Boomi operations team relies on with respect to the Boomi Public and Private Managed Atom Clouds.  We monitor the cloud as a whole, versus drill down too far into individual processes and process connections.


The '**Atom Worker**' JVMs which handle real-time, web service integration processes.  These are spun up by the procworker.sh shell script and run for 24 hours.  And then continually renewed for another 24 hours. Each Atom Worker JVM sits there and services multiple requests simultaneously (configurable).
This is what SMEs are focused on.  It looks very promising in terms of exposing telemetry data related to real-time processing.  Including, potentially, the ability to 'see inside' the process.  We have verified that Customers uses Atom Workers extensively.  We counted as many as 26 Atom Workers attached to one of customer's production clouds.  That cloud has 10 nodes.  So it would be a matter of attaching a New Relic Agent to each Atom Worker instance.

So, '**Atom Node**' and '**Atom Worker**' where New Relic Agent needs to be deployed.


# STEP1 : Deployment at '**Atom Node**'

## Apply Java agent and give App Name

Look for atom.vmoptions in your atom node installation and update javaagent [ _The absolute path must point to your newrelic.jar within the agent installation location_ ] and app name with sudo privileges 
eg. /opt/Boomi_AtomSphere/Cloud/Cloud_atom_cloud_01/bin/atom.vmoptions and add the following options

-Dnewrelic.config.app_name=BOOMI_EU_PROD_ATOM
-javaagent:/home/ec2-user/nr_agent/newrelic/newrelic.jar

# STEP 2: Deployment at '**Atom Worker**'

## Apply Java agent and give App Name

Look for procworker.sh in your atom node installation and carefully update javaagent and app name with sudo privileges 
eg. /opt/Boomi_AtomSphere/Cloud/Cloud_atom_cloud_01/bin/procworker.sh
-Dnewrelic.config.app_name=BOOMI_EU_PROD__WORKER
-javaagent:/home/ec2-user/nr_agent/newrelic/newrelic.jar

# STEP 3: Updated the existing newrelic agent installation with following instrumented jars

