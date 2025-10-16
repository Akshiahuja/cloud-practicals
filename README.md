# cloud-practicals
________________________________________
Practical 1: Virtual Cloud Lab 
Aim: To gain hands-on experience with a cloud environment by signing up for a virtual cloud lab 
(e.g., Google Cloud Skills Boost) and provisioning a basic cloud resource within its provided 
isolated environment. 
Procedure: 
1. Access Cloud Skills Boost: 
○ Open a web browser and navigate to https://www.cloudskillsboost.google/. 
2. Sign In/Sign Up: 
○ Click "Sign In" and use your Google account to log in. Create a new Google 
account if you don't have one. 
3. Find and Start a Lab: 
○ Use the search bar to find an introductory lab, such as "Compute Engine: Qwik 
Start" or "Cloud Storage: Qwik Start." 
○ Select a free lab or one that is part of a free Quest. 
○ Click "Start Lab" on the chosen lab's page. 
○ Wait for the temporary credentials (Username and Password) for the isolated 
Google Cloud project to appear on the left panel. 
4. Open Google Cloud Console: 
○ Click the "Open Google Console" button from the lab panel. This will open a new 
browser tab. 
○ Copy the provided "Username" and "Password" from the lab panel and paste them 
into the Google sign-in prompts in the new tab. Accept any terms and conditions. 
5. Provision a Resource (Example: Create a Virtual Machine): 
○ In the Google Cloud Console, use the Navigation menu (three horizontal lines) or 
the search bar at the top. 
○ Search for and navigate to "Compute Engine" -> "VM instances." 
○ Click the "+ Create Instance" button. 
○ Configure VM: 
■ Name: my-lab-vm 
■ Region/Zone: Keep the default or as specified by the lab. 
■ Machine configuration: Select a free-tier eligible type like e2-micro or 
f1-micro. 
■ Boot disk: Keep the default operating system (e.g., Debian or Ubuntu). 
■ Firewall: Check "Allow HTTP traffic" and "Allow HTTPS traffic." 
○ Click "Create." 
6. Verify Provisioning: 
○ Observe the VM instances list until my-lab-vm shows a green checkmark status, 
indicating it's running. 
7. End the Lab: 
○ Return to the Google Cloud Skills Boost tab and click the "End Lab" button to clean 
up all temporary resources. 
Result: Successfully signed up for a Google Cloud Skills Boost lab, accessed the temporary 
Google Cloud Console, and provisioned a e2-micro (or f1-micro) virtual machine instance within 
the provided lab environment. This demonstrated the fundamental process of resource creation 
in a cloud setting without impacting personal accounts. 
________________________________________
Practical 2: Create a Cloud-based Virtual Machine and Deploy 
Aim: To create a persistent virtual machine instance on a cloud platform (Google Cloud Platform 
in this case) and perform a basic application deployment (installing a web server). 
Procedure: 
1. Access Google Cloud Platform: 
Go to https://cloud.google.com/ and log in to your GCP account. Ensure billing is 
enabled for your project (you'll use your free tier credits). 
2. Create a New GCP Project (if necessary): 
In the GCP Console, click the project dropdown at the top. 
Select "New Project," provide a name (e.g., MyPersistentVMProject), and click 
"Create." 
Ensure this new project is selected. 
3. Navigate to Compute Engine: 
From the Navigation menu, go to "Compute Engine" -> "VM instances." 
4. Create a VM Instance: 
Click "+ Create Instance." 
Name: my-permanent-web-vm 
Region and Zone: Select a region close to India (e.g., asia-south1-a). 
Machine configuration: Set Series to E2 and Machine type to e2-micro (part of 
the Always Free tier). 
Boot disk: Select Debian GNU/Linux 11 (bullseye) (or Ubuntu LTS) and set Size 
(GB) to 30 GB (within free tier limits). Click "Select." 
Firewall: Check both "Allow HTTP traffic" and "Allow HTTPS traffic." 
Click "Create." 
5. Connect via SSH: 
Once the VM is running (green checkmark), locate the "SSH" dropdown next to 
your instance. 
Select "Open in browser window" to launch a secure terminal. 
6. Deploy Nginx Web Server: 
In the SSH terminal, execute the following commands: 
sudo apt update # Update package lists             
sudo apt install nginx -y  # Install Nginx web server        
sudo systemctl status nginx     # Verify Nginx is running  
(press 'q' to exit) 
7. Verify Deployment: 
Go back to the VM instances page in the GCP Console. 
Copy the "External IP" address of your my-permanent-web-vm. 
Open a new browser tab and paste the External IP. 
8. Clean Up: 
Return to "Compute Engine" -> "VM instances." 
Check the box next to my-permanent-web-vm. 
Click "Delete" and confirm the action. 
Result: A e2-micro virtual machine named my-permanent-web-vm was successfully created in 
the asia-south1 region. Nginx web server was installed and configured, allowing access to its 
default welcome page via the VM's external IP address. This demonstrates the end-to-end 
process of VM provisioning and a basic application deployment on Google Cloud Platform. 
________________________________________
Practical 3: Create a Cloud Storage Bucket and Host a Static Website 
Aim: To create a Google Cloud Storage bucket, configure it for public access, and host a simple 
static website using the bucket's static website hosting feature. 
Procedure: 
1. Prepare Static Website Files: 
○ Create a local folder (e.g., my-website-files). 
○ Inside, create index.html: 
<!-- <!DOCTYPE html> 
<html> 
<head><title>My GCP Static Site</title></head> 
<body><h1>Welcome to my GCP hosted website!</h1><p>This page 
is served directly from a Cloud Storage bucket.</p></body> 
</html>  -->
○ (Optional) Create 404.html for custom error pages. 
2. Access Cloud Storage: 
○ Log in to the GCP Console. 
○ From the Navigation menu, go to "Cloud Storage" -> "Buckets." 
3. Create a Storage Bucket: 
○ Click "+ Create Bucket." 
○ Name your bucket: Enter a globally unique name (e.g., 
your-unique-website-bucket-2025). 
○ Choose where to store your data: Select "Region" and a region close to India 
(e.g., asia-south1). 
○ Choose a default storage class: Select "Standard." 
○ Choose how to control access to objects: Select "Uniform." Crucially, ensure 
"Enforce public access prevention on this bucket" is unchecked or uncheck it 
if present. 
○ Click "Create." 
4. Upload Website Files: 
○ Click on the newly created bucket to enter its details. 
○ Click "Upload files" and select index.html (and 404.html) from your local folder. 
5. Make Objects Public: 
○ In your bucket details, go to the "Permissions" tab. 
○ Click "Grant Access." 
○ New principals: Type allUsers. 
○ Select a role: Choose "Cloud Storage" -> "Storage Object Viewer." 
○ Click "Save." Confirm "Allow public access" if prompted. 
6. Configure Static Website Hosting: 
○ Go to the "Configuration" tab of your bucket. 
○ Click "Add website configuration" or "Edit website configuration." 
○ Index page suffix: Enter index.html. 
○ Error page suffix: Enter 404.html (if you created one). 
○ Click "Save." 
7. Test the Website: 
○ On the "Configuration" tab, find the "Website URL" (e.g., 
https://storage.googleapis.com/your-unique-website-bucket-2025/). 
○ Copy this URL and paste it into a new browser tab to view your hosted website. 
8. Clean Up: 
○ Return to "Cloud Storage" -> "Buckets." 
○ Check the box next to your website bucket. 
○ Click "Delete" and type the bucket name to confirm. 
Result: A Google Cloud Storage bucket was successfully created and configured to serve a 
static website. The index.html file was uploaded and made publicly accessible, allowing the 
website to be viewed directly via a storage.googleapis.com URL. This demonstrates a 
cost-effective and highly scalable method for static content hosting. 
________________________________________
Practical 4: Connect with Google Cloud DB Service and Run SQL 
Queries 
Aim: To provision a fully managed relational database instance (Google Cloud SQL for 
MySQL), establish a connection, and execute fundamental SQL queries for data definition, 
manipulation, and basic analysis. 
Procedure: 
1. Access Cloud SQL: 
○ Log in to the GCP Console. 
○ From the Navigation menu, go to "SQL." 
2. Create a Cloud SQL Instance: 
○ Click "+ Create Instance." 
○ Choose "MySQL." 
○ Instance ID: my-gcp-mysql-instance (unique name). 
○ Password: Set a strong root password and remember it. 
○ Region: Select a region close to India (e.g., asia-south1). 
○ Choose a configuration: Select "Development." 
○ Machine type: Shared-core (e2-micro) is suitable and free-tier eligible. 
○ Click "Create Instance." (Allow several minutes for creation). 
3. Configure Network Access: 
○ Once the instance is ready, click on its name (my-gcp-mysql-instance) to view 
details. 
○ In the left navigation, click "Connections." 
○ Go to the "Networking" tab. 
○ Click "+ Add a network." 
○ Name: My Home IP. 
○ Network: Enter your current public IP address followed by /32 (e.g., 
123.45.67.89/32). (You can find your public IP by searching "what is my ip" on 
Google). 
○ Click "Done," then "Save." 
4. Connect via Cloud Shell: 
○ Click the "Activate Cloud Shell" icon in the GCP Console (top right). 
○ Once Cloud Shell is ready, execute the connection command: 
gcloud sql connect my-gcp-mysql-instance --user=root 
--database=mysql 
○ When prompted, enter the root password you set for the instance. 
○ You should now see the mysql> prompt. 
5. Run SQL Queries and Analyze: 
○ Create a new database: 
CREATE DATABASE customer_db; 
○ Switch to the new database: 
USE customer_db; 
○ Create a table: 
CREATE TABLE customers ( 
customer_id INT AUTO_INCREMENT PRIMARY KEY, 
first_name VARCHAR(50) NOT NULL, 
last_name VARCHAR(50) NOT NULL, 
email VARCHAR(100) UNIQUE, 
registration_date DATE 
); 
○ Insert data: 
INSERT INTO customers (first_name, last_name, email, 
registration_date) VALUES 
('Alice', 'Smith', 'alice@example.com', '2023-01-15'), 
('Bob', 'Johnson', 'bob@example.com', '2023-03-20'), 
('Charlie', 'Brown', 'charlie@example.com', '2024-02-10'), 
('Diana', 'Prince', 'diana@example.com', '2023-05-01'); 
○ Retrieve all customers: 
SELECT * FROM customers; 
○ Retrieve customers registered after a specific date: 
SELECT first_name, last_name FROM customers WHERE 
registration_date > '2023-04-01'; 
○ Count total customers: 
SELECT COUNT(*) AS total_customers FROM customers; 
○ Exit MySQL prompt: 
exit; 
○ Close Cloud Shell: Type exit again. 
6. Clean Up: 
○ Return to "SQL" in the GCP Console. 
○ Check the box next to my-gcp-mysql-instance. 
○ Click "Delete Instance" and type the instance ID to confirm. 
Result: A Google Cloud SQL for MySQL instance was successfully provisioned in the 
asia-south1 region. Connection was established via Cloud Shell, and a new database and table 
were created. Standard SQL queries for data insertion, retrieval, and basic aggregation (like 
COUNT) were executed, demonstrating the use of a managed database service.
________________________________________
Practical 5: Create your Amazon Bucket for Cloud Storage

Aim: To create an Amazon S3 (Simple Storage Service) bucket for object storage, a foundational service used for hosting static websites, backups, and media files.
Procedure:
1.	Log in to the AWS Console: Access the AWS Management Console and search for "S3" to navigate to the service dashboard.
2.	Create a Bucket:
○	Click the Create bucket button.
○	Choose a globally unique name for your bucket (e.g., my-first-aws-bucket-sept2025).
○	Select an AWS Region (e.g., Asia Pacific (Mumbai)). This is important for data residency and latency.
○	Leave all other settings at their defaults for this practical.
○	Click Create bucket.
3.	Upload Objects:
○	Once the bucket is created, click on its name.
○	Click Upload to add files from your local computer. You can drag and drop or use the file picker.
○	You can upload a simple image or a text file.
4.	Manage Access:
○	By default, S3 objects are private. To make a file public, select it in the bucket, click the Actions dropdown, and choose Make public using ACL. Confirm the action.
○	You can then access the file's URL from the object's properties tab.
Result: A new Amazon S3 bucket was successfully created in the Mumbai region. Objects were uploaded to the bucket and their public access was configured, demonstrating how S3 can be used for simple and secure cloud storage.
________________________________________
Practical 6: Create your Amazon EC2 Instances for Windows and Linux

Aim: To launch two distinct virtual machines on Amazon EC2: one running Linux and one running Windows Server, showcasing EC2's versatility in provisioning different operating systems.
Procedure:
1.	Access the EC2 Dashboard: Log in to the AWS Management Console and search for "EC2" to open the dashboard.
2.	Launch the Linux Instance:
○	Click the Launch instance button.
○	Give the instance a name (e.g., my-linux-server).
○	Under Application and OS Images (Amazon Machine Image), choose a free tier-eligible Linux AMI, such as Ubuntu Server 22.04 LTS.
○	For Instance type, select t2.micro (free tier eligible).
○	Create a new key pair to securely connect via SSH.
○	Click Launch instance.
3.	Launch the Windows Instance:
○	Repeat the process by clicking Launch instance again.
○	Name this instance my-windows-server.
○	Under Application and OS Images, choose a Microsoft Windows Server AMI (free tier eligible).
○	Select the t2.micro instance type.
○	Use an existing key pair or create a new one.
○	Click Launch instance.
4.	Connect to the Instances:
○	Linux: Select your Linux instance, click Connect, and follow the instructions to connect via SSH using your key pair.
○	Windows: Select your Windows instance, click Connect, then go to the RDP client tab. Click Get password and upload your private key file to decrypt and retrieve the administrator password. Use a Remote Desktop client (like Microsoft RDP Client) to connect.
5.	Clean Up: To avoid unwanted charges, make sure to terminate both instances after you are finished. Select them in the EC2 dashboard, click Instance state, and then Terminate instance.
Result: Two separate virtual machines, one running Ubuntu Linux and the other running Windows Server, were successfully provisioned and accessed on Amazon EC2. This practical illustrates the flexibility of using EC2 to deploy various operating system environments on-demand.
________________________________________
Practical 7: Build a Container Application and Upload on a cloud platform of your choice

Aim: To containerize a simple application using Docker and upload the resulting image to a cloud-based container registry, preparing it for scalable and portable deployment.
Procedure:
1.	Create a Sample Application and Dockerfile:
○	On your local machine, create a new folder and a simple application file (e.g., app.py for Python).
○	In the same folder, create a file named Dockerfile. This file contains the instructions for building your container image.
2.	Build the Docker Image:
○	Open a terminal or command prompt in the same directory.
○	Use the docker build command to create the image. It's crucial to tag it for the cloud registry:
Bash
docker build -t gcr.io/your-project-id/my-web-app:v1 .

○	Replace your-project-id with your actual Google Cloud project ID.
3.	Authenticate to the Container Registry:
○	You need to authenticate your local Docker client to the cloud registry. For Google Cloud, use the following command:
Bash
gcloud auth configure-docker

4.	Push the Image to the Registry:
○	Use the docker push command to upload your tagged image to the cloud registry.
Bash
docker push gcr.io/your-project-id/my-web-app:v1

Result: A sample application was successfully packaged into a Docker container image. The image was then authenticated and pushed to a cloud container registry (Google Container Registry in this example), making it available for deployment on various cloud services.
________________________________________
Practical 8: Deploy and Monitor Web App using Azure App service

Aim: To deploy a web application to Azure App Service, a Platform as a Service (PaaS) offering, and utilize its built-in monitoring tools to observe the application's performance.
Procedure:
1.	Create an App Service:
○	Log in to the Azure Portal.
○	Search for "App Services" and click Create.
○	Provide a unique name for your web app, choose a runtime stack (e.g., Node.js 18 LTS), and select a region (e.g., Central India).
○	Click Review + create and then Create.
2.	Deploy the Application:
○	Once the App Service is created, go to its overview page.
○	In the left-hand menu, select Deployment Center.
○	Choose a source, such as GitHub, to connect your code repository.
○	Follow the wizard to configure a CI/CD pipeline that automatically deploys your code. Alternatively, you can use a manual deployment via FTP or a local Git repository.
3.	Monitor the App:
○	After deployment, navigate to the Monitoring section in the App Service menu.
○	You can explore various metrics like Data in/out, Requests, and CPU Time.
○	Go to Live Metrics Stream to see real-time performance data and diagnostic information as users interact with your application.
Result: A web application was successfully deployed to Azure App Service without manual server configuration. The platform's native monitoring tools were used to visualize performance data and gain insights into the application's health.
________________________________________
Practical 9: Sign Up and Explore Azure ML Studio. Create and Run at least one Azure ML Pipeline interface

Aim: To explore the Azure Machine Learning (ML) Studio and create a simple automated ML pipeline using its graphical, no-code/low-code interface.
Procedure:
1.	Create an Azure ML Workspace:
○	Log in to the Azure Portal and search for "Machine Learning."
○	Click Create and follow the steps to create a new workspace.
○	Once created, click Launch Studio to open the Azure ML Studio web interface.
2.	Upload a Dataset:
○	In the studio, navigate to Data and click Create.
○	Upload a sample dataset, such as a CSV file for a classification problem (e.g., Iris or Titanic dataset).
3.	Create and Run an Automated ML Job:
○	On the left-hand menu, select Automated ML.
○	Click New automated ML job.
○	Select your uploaded dataset and click Next.
○	Choose the Target column (the feature you want to predict).
○	Select the task type (e.g., Classification or Regression).
○	Click Finish to submit the job. The system will automatically run multiple experiments to find the best model.
4.	Review the Results:
○	Once the job is complete, review the Run details.
○	Go to the Models tab to see a leaderboard of the models that were trained, sorted by their performance metrics.
Result: An Azure ML workspace was set up, and a dataset was prepared. A classification pipeline was created and executed using the automated ML feature, which trained and evaluated multiple models without manual coding, demonstrating the power of Azure ML Studio for rapid model development.
________________________________________
Practical 10: Visualize your data using Amazon QuickSight

Aim: To use Amazon QuickSight, a business intelligence service, to connect to an Amazon data source and generate various data visualizations for analysis.
Procedure:
1.	Access QuickSight:
○	Log in to the AWS Management Console and navigate to QuickSight. You may need to sign up for an account if it's your first time.
2.	Connect to a Data Source:
○	In QuickSight, click New analysis and then New data set.
○	Select a data source. For this practical, choose S3 or Athena if you have data there. Alternatively, use one of the sample datasets.
3.	Create an Analysis:
○	After connecting to your data, you will be taken to the analysis page.
○	On the left, you'll see a list of data fields.
○	Choose a visual type from the bottom menu (e.g., a bar chart or pie chart).
○	Drag and drop the data fields onto the relevant axes and fields of the visualization. For example, drag a Category field to the X-axis and a Sales field to the Y-axis.
4.	Create a Dashboard:
○	Once you have created several visualizations, click Share -> Publish dashboard.
○	Give your dashboard a name and publish it to share with others.
Result: A connection was established to an Amazon data source, and a dataset was created within QuickSight. Different data visualizations, such as bar charts and line graphs, were generated to explore the data, showcasing how QuickSight can be used to create informative dashboards and reports.
________________________________________
Practical 11: Develop, Build and Deploy a container Application On Google Compute Engine

Aim: To deploy a containerized application directly onto a Google Compute Engine virtual machine, using its native support for container deployment.
Procedure:
1.	Prepare and Push a Docker Image:
○	Create a simple Docker image for a web application and push it to Google Container Registry (GCR) or Artifact Registry. This step is identical to Practical 7.
2.	Create a Compute Engine VM:
○	Navigate to Compute Engine -> VM instances in the GCP Console.
○	Click Create Instance.
○	Give your VM a name (e.g., my-container-vm).
○	Under the Container section, check the box Deploy a container image to this VM instance.
3.	Configure Container Deployment:
○	Enter the Container image URL of the image you pushed to GCR (e.g., gcr.io/your-project-id/my-web-app:v1).
○	You can also configure port mappings if your application listens on a specific port.
4.	Launch the VM:
○	Ensure you have allowed HTTP and HTTPS traffic in the firewall settings.
○	Click Create. The VM will automatically be created and the container will be deployed and started.
5.	Verify Deployment:
○	Once the VM is running, copy its External IP address.
○	Paste the IP into a browser to access your containerized application.
○	You can also connect via SSH and use the docker ps command to see the running container.
Result: A Google Compute Engine VM was successfully provisioned, and a containerized application was automatically deployed onto it from a container registry. The application was made accessible via the VM's external IP, demonstrating the ease of deploying containers on a Compute Engine instance.
