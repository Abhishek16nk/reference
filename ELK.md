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

sudo yum install -y java-1.8.0-amazon-corretto-devel


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


==================================================================================================================

Security group:

To ensure that your EC2 instance hosting the ELK stack allows traffic on the necessary ports (9200 for Elasticsearch and 5601 for Kibana), you'll need to modify the security group associated with that EC2 instance. Here are the steps to do this:

Step 1: Identify Your EC2 Instance Security Group
Log in to the AWS Management Console.
Navigate to the EC2 Dashboard.
Find your EC2 instance running the ELK stack in the Instances section.
Select the instance and look for the Security Groups associated with it in the description tab at the bottom.

Step 2: Modify the Security Group
Click on the security group link to open its details.
Go to the Inbound Rules tab.
Click on the Edit inbound rules button.

Step 3: Add Rules for Elasticsearch and Kibana
Add Rule for Elasticsearch:

Click on Add rule.
Type: Custom TCP
Protocol: TCP
Port range: 9200
Source: Choose either:
My IP: if you want to allow access only from your IP address.
Custom: Enter the CIDR range of IPs you want to allow access (e.g., 0.0.0.0/0 for all IPs, but this is not recommended for production).
Description: (optional) Add a note like "Allow Elasticsearch access".

Add Rule for Kibana:

Click on Add rule.
Type: Custom TCP
Protocol: TCP
Port range: 5601
Source: Choose the same option as above (e.g., your IP, or a specific CIDR range).
Description: (optional) Add a note like "Allow Kibana access".

Step 4: Save Changes
After adding the rules, click on the Save rules button to apply the changes.

after installing kibana and elastic search:
 
Step 5: Verify Connectivity
After the rules are saved, verify that you can access Elasticsearch and Kibana from your browser using:
For Elasticsearch: http://<your-ec2-public-ip>:9200
For Kibana: http://<your-ec2-public-ip>:5601
