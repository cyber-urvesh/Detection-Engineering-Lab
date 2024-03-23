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

**Note - the built-in elastic user password would be displayed directly in your terminal.** 

Once the installation is completed, we need to edit the elasticsearch.yml file located within /etc/elasticsearch/elasticsearch.yml. We need to make the necessary changes to ensure that Elasticsearch is running smoothly as per our lab requirements. 

    sudo nano /etc/elasticsearch/elasticsearch.yml

You can note down the following configuration parameters that requires changes within the config file: 

    network.host: <your IP here> 
    xpack.security.enabled: true
    xpack.security.enrollment.enabled: true
    xpack.security.http.ssl.enabled: false
    Comment -- #keystore.path: certs/http.p12

If you wish to just directly copy paste the config that I am using\, here it is: 

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

**Make sure you replace < YOUR-IP > with your own IP address where you want to host elasticsearch.** 
Once the above changes are completed, start the Elasticsearch service using the below commands. 
To configure Elasticsearch to start automatically when the system boots up, run the following commands:

    sudo /bin/systemctl daemon-reload
    sudo /bin/systemctl enable elasticsearch.service

Elasticsearch can be started and stopped as follows:

    sudo systemctl start elasticsearch.service
    
## Install Kibana 
Kibana is included within the same Elasticsearch DEB repo. You can directly install Kibana using the command: 

    sudo apt install kibana 
Generate a password for "kibana_system" user 

    sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system

**Further steps - Construction in progress XD lol ..** 

## Generate Encryption Key for Xpack 
Run the following command to generate a random key: 

    openssl rand -base64 32

This command generates a random string encoded in base64 that is 32 bytes long, suitable for use as your encryption key. Now open the Kibana config file and make the necessary changes. You need to add the generated encryption key to your kibana.yml. 

    xpack.encryptedSavedObjects.encryptionKey: "your-encryption-key"

Save your changes and restart the Kibana service 

    sudo systemctl restart kibana
