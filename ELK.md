Step 4: Install ELK Stack on EC2 Instance
Launch an EC2 Instance:

Choose an instance type (e.g., t2.micro) with Amazon Linux or Ubuntu.
Ensure security groups allow necessary traffic (port 9200 for Elasticsearch, port 5601 for Kibana).
SSH into the EC2 Instance:

bash
Copy code
ssh -i "your-key.pem" ec2-user@your-ec2-instance-ip
-------------------------------------------------------
Install Java (required for Elasticsearch):

bash
Copy code
sudo yum install java-1.8.0-openjdk-devel -y

-----------------------------------------------------------------------
Download and Install Elasticsearch:

bash
Copy code
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.x.x-x86_64.rpm
sudo rpm --install elasticsearch-7.x.x-x86_64.rpm
Configure Elasticsearch: Edit the configuration file to bind to your EC2 public IP or 0.0.0.0 in /etc/elasticsearch/elasticsearch.yml:

yaml
Copy code
network.host: 0.0.0.0
----------------------------------------------
Start Elasticsearch:

bash
Copy code
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch

------------------------------------------------------------------------
Install Kibana:

bash
Copy code
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.x.x-x86_64.rpm
sudo rpm --install kibana-7.x.x-x86_64.rpm
Configure Kibana: Edit /etc/kibana/kibana.yml:

yaml
Copy code
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]

-----------------------------------------------------------
Start Kibana:

bash
Copy code
sudo systemctl start kibana
sudo systemctl enable kibana
