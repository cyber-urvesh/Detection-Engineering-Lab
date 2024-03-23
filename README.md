# Detection-Engineering-Lab
A repo focusing on creation of home lab for Detection Engineering

## Install Elasticsearch
Download and install Elasticsearch using the below commands:

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg


You may need to install the  `apt-transport-https`  package on Debian before proceeding:

    sudo apt-get install apt-transport-https

Save the repository definition to  `/etc/apt/sources.list.d/elastic-8.x.list`:

    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main"  | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

You can install the Elasticsearch Debian package with:

    sudo apt-get update && sudo apt-get install elasticsearch

Once the installation is completed, we need to edit the elasticsearch.yml file located within /etc/elasticsearch/elasticsearch.yml. We need to make the necessary changes to ensure that Elasticsearch is running smoothly as per our lab requirements. 

    sudo nano /etc/elasticsearch/elasticsearch.yml

You can note down the following configuration parameters that requires changes within the config file: 

    network.host: <your IP here> 
    xpack.security.enabled: true
    xpack.security.enrollment.enabled: true
    xpack.security.http.ssl.enabled: false
    Comment -- #keystore.path: certs/http.p12

If you wish to just directly copy paste the config that I am using\, here it is : 

    # ======================== Elasticsearch Configuration ========================
    # Path to directory where to store the data (separate multiple locations by comma):
    path.data: /var/lib/elasticsearch
    # Path to log files:
    path.logs: /var/log/elasticsearch
    # ---------------------------------- Network -----------------------------------
    network.host: <YOUR-IP> 
    # By default Elasticsearch listens for HTTP traffic on the first free port it
    #http.port: 9200
    #----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
    # Enable security features
    xpack.security.enabled: true
    xpack.security.enrollment.enabled: true
    # Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
    xpack.security.http.ssl.enabled: false
    # keystore.path: certs/http.p12
    # Enable encryption and mutual authentication between cluster nodes
    
    xpack.security.transport.ssl:
    enabled: true
    verification_mode: certificate
    keystore.path: certs/transport.p12
    truststore.path: certs/transport.p12
    cluster.initial_master_nodes: ["ELKStack"]
    # Allow HTTP API connections from anywhere
    # Connections are encrypted and require user authentication
    
    http.host: 0.0.0.0
    # Allow other nodes to join the cluster from anywhere
    # Connections are encrypted and mutually authenticated
    #transport.host: 0.0.0.0
    #----------------------- END SECURITY AUTO CONFIGURATION -------------------------

Make sure you replace < YOUR-IP > with your own IP address where you want to host elasticsearch. 
