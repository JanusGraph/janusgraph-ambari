# JanusGraph Ambari plugin
JanusGraph is a scalable graph database optimized for storing and querying graphs containing hundreds of billions of vertices and edges distributed across a multi-machine cluster. JanusGraph is a transactional database that can support thousands of concurrent users executing complex graph traversals in real time. This plugin is designed to make it easier for existing Ambari users to integrate JanusGraph and manage it as a native service. 

Instructions on how to add JanusGraph to [Apache Ambari](https://ambari.apache.org ) or [Hortonworks Data Platform(HDP)](https://hortonworks.com/products/data-platforms/hdp/) as a service can be found below.

## Requirements
- This plugin has been tested with Ambari versions 2.5, and 2.6. It is not compatible with Ambari version 2.7.
- The JanusGraph ambari plugin expects Apache Solr to be available. You need to install the lucidworks-hdpsearch package or mpack to use janusgraph-ambari. If you wish to use the plugin without Solr please look under the known issues section.
- The Ambari Infra Solr Client is used for Solr checks so the Ambari Infra Solr service needs to be installed as well.
- HBase and Spark2 are also required dependancies.

## Plugin Installation

- To download the service and link it to Ambari run the following commands
```
VERSION=`hdp-select versions | head -n1 | cut -d'.' -f1,2`
git clone https://github.com/JanusGraph/janusgraph-ambari.git /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/JANUSGRAPH
```

```
sudo service ambari-server restart
```
## Service Installation
- Once the ambari-server service has restarted login into the webui and click on the Actions dropdown on the lower lefthand side and select '+Add Service'.
Click on the checkbox next to JanusGraph and then press next. 
Currently JanusGraph needs to be located on the same host as Solr. If Solr is not yet installed you'll need to install it through the [mpack](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.3/bk_solr-search-installation/content/ch_hdp-search-install-ambari.html).

- Click add service from the actions dropdown:
![Image](../master/screenshots/add_service.png?raw=true)
- The add service wizard will show all available services:
![Image](../master/screenshots/add_service_wizard.png?raw=true)
- Scroll down and check the box next to JanusGraph and click next. If Solr is not already installed it will be automatically added as a dependancy:
![Image](../master/screenshots/add_service_wizard_select_JG.png?raw=true)
- The assign masters screen shows you where all of your existing services are installed. You'll need to scroll down to find JanusGraph:
![Image](../master/screenshots/assign_masters.png?raw=true)
- Select the host you want to install JanusGraph on from the dropdown. It's reccomended to install it on the same host as Solr for performance:
![Image](../master/screenshots/add_service_wizard_JG.png?raw=true)
- There is currently no JanusGraph client so the Assign Slaves and Clients screen will be bypassed. Next you'll be able to customize your installation. No user supplied values are required:
![Image](../master/screenshots/customize_services.png?raw=true)
- You can customize environment details like target directories and your Gremlin Server configuration:
![Image](../master/screenshots/customize_services_jg-env.png?raw=true)
- Click Deploy on the Installation review screen:
![Image](../master/screenshots/review.png?raw=true)
- Once the JanusGraph installation has been successfully complete the sevices view should look like this:
![Image](../master/screenshots/janusgraph_installed.png?raw=true)

## Known Issues
- If JanusGraph and Solr are installed at the same time both services may fail to start during install. You can simply start the Solr service and then start the JanusGraph service after install to resolve. 
- The plugin won't install without Solr. If you wish to use the plugin without Solr, and mixed index support, you can edit the metainfo.xml file found at `/var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/JANUSGRAPH` and comment out, or remove, SOLR from the required Services.
- If you are using the Docker based sandbox you will need to add a port mapping for janusgraph on port 8182 to allow connectivity. You can add `-p 8182:8182 \` to `sandbox/proxy/proxy-deploy.sh` and then run the script to redploy the proxy with the added port mapping.

## License

JanusGraph Ambari plugin is provided under the [Apache 2.0
license](APACHE-2.0.txt) and documentation is provided under the [CC-BY-4.0
license](CC-BY-4.0.txt). For details about this dual-license structure, please
see [`LICENSE.txt`](LICENSE.txt).
