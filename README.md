
# Deploying Web-pages on Virtual Machines

## There are three web pages to be deployed:
1. The home page is the default page (VM2)
2. The upload page is where you can upload the files to your Azure Blob Storage (VM1)
3. The error page for 403 and 502 errors
#### Application Gateway has to be configured in the following manner:
1. Example.com should be pointed to the home page
2. Example.com/upload should be pointed to the upload page
3. Application Gateway’s error pages should be pointed to error.html which should be hosted as a static website in Azure Containers. The error.html file is present in the GitHub repository
![Screenshot 2024-05-04 113322](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/0c4f1741-8ca7-4d55-9753-f5abbec73570)




Azure VMs (Virtual Machines) are a fundamental service offered by Microsoft Azure, its cloud computing platform. 

#### What are Azure VMs?

Virtual machines are essentially virtual computers hosted on Microsoft's powerful physical servers.
They provide a dedicated computing environment within the Azure cloud, offering the flexibility of a physical machine without the need to manage the underlying hardware.
Each VM comes with its own CPU, memory, storage, and operating system, allowing you to install and run applications just like on a physical computer.

#### Key Features of Azure VMs:

1.Scalability: Easily scale your resources up or down based on your needs. Add or remove VMs as your workload fluctuates.

2.Cost-Effectiveness: Pay only for the resources you use, optimizing costs compared to maintaining physical servers.

3.High Availability: Azure offers various options for ensuring high availability of your VMs, minimizing downtime.

4.Security: Benefit from Microsoft's robust security infrastructure and features to protect your VMs and data.

5.Wide Range of Operating Systems: Choose from a vast selection of Windows and Linux distributions to suit your specific needs.

6.Integration with Azure Services: Seamlessly integrate your VMs with other Azure services like databases, storage solutions, and networking tools.
#### Common Use Cases for Azure VMs:
1.Development and Testing: Create isolated environments for developing and testing applications without impacting production systems.

2.Hosting Web Applications: Deploy and run web applications on scalable and secure VMs.

3.Running Business-Critical Applications: Migrate existing business applications to the cloud for enhanced performance and reliability.

4.Disaster Recovery: Set up VMs for disaster recovery purposes, ensuring business continuity in case of outages.


#### Azure Application Gateway 
Is a powerful web traffic load balancer operating at the application layer (layer 7) within the Azure cloud platform. Here's a breakdown of its key features and functionalities:

#### What it does:

1.Load Balancing: Distributes incoming traffic across multiple servers in your web application infrastructure, ensuring high availability and scalability.

2.Application Layer Routing: Makes routing decisions based on various factors like URL path, host header, or other HTTP(S) request attributes. This allows you to direct specific traffic to the most appropriate backend servers.

3.SSL/TLS Termination: Offloads the encryption and decryption process from your backend servers, improving performance and security.

4.Web Application Firewall (WAF): Provides basic protection against common web attacks like SQL injection and cross-site scripting.

#### Common Use Cases Application Gateway:

1.Distributing traffic across multiple web servers: Ensure optimal performance and prevent overloading individual servers.

2.Routing traffic based on specific criteria: Direct users to different backend servers based on their requests.

3.Securing web applications: Mitigate common web attacks with the built-in WAF.

4.Offloading SSL/TLS termination: Improve backend server performance and simplify certificate management.

#### Azure Traffic Manager 
Is a DNS-based traffic load balancer offered by Microsoft Azure. It plays a crucial role in managing and optimizing traffic flow within your applications. Here's a breakdown of its key features and functionalities:

#### What it does:

1.Traffic Routing: Directs user traffic to the most appropriate service endpoint based on various pre-defined routing methods. This can be based on factors like:

- Priority: Route traffic to the highest priority endpoint.
- Performance: Route traffic to the endpoint with the fastest response times.
- Geographic: Route traffic to the endpoint closest to the user's location.
- Weighted Round Robin: Distribute traffic evenly across multiple endpoints based on weight assignments.
- Subnet: Route traffic based on the user's IP address subnet.
- Multi-value: Return multiple healthy endpoints in the DNS response for further client-side selection.
2.Health Monitoring: Continuously monitors the health of your service endpoints, automatically detecting and responding to failures.

3.High Availability: Ensures uninterrupted service by directing traffic away from unhealthy endpoints to healthy ones.

4.Global Reach: Supports distributing traffic across multiple Azure regions and even external endpoints outside Azure.

#### Key Benefits:

1.Improved Application Performance: Ensures users are directed to the most responsive and geographically closest endpoint.

2.Reduced Downtime: Automatic failover to healthy endpoints minimizes service disruptions.

3.Scalability: Easily scales your application infrastructure by adding or removing endpoints as needed.

4>Simplified Management: Provides a centralized interface for configuring traffic routing and monitoring endpoints.

#### Common Use Cases Azure Traffic Manager:

1>Distributing traffic across geographically dispersed web servers: Improve user experience by reducing latency.

2>Ensuring high availability for critical applications: Minimize downtime and maintain service continuity.

3>Implementing disaster recovery strategies: Route traffic away from failing regions to ensure business continuity.

4>Balancing traffic across multiple cloud services: Optimize resource utilization and prevent overloading individual services.

# Let's Start implementation
Prerequisite : Azure account

Mind map for Deploying Web pages![Screenshot 2024-05-04 081555](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/3155ffb1-e96a-4ad1-9858-5ad7fb811e40)

# Step1 : Creating Virtual Machines:
- Sign in to the Azure portal: Go to https://portal.azure.com and sign in with your Azure account.
- Choose your subscription.
- Create a new resource group or select an existing one.
- Search for "Virtual Machines": In the search bar, type "Virtual Machines" and select the service.
- Click "Create" and choose "Virtual machine" then give vm name CentralusVM-1
- Select a region as Central US
- Select image as Ubuntu Server 22.04 LTS


![Screenshot (1101)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/69f4657a-1d45-4e18-b78f-460b84fa6364)

- Select Authentication type as password and give user nanme and password.
- Select public inbound port as Allow
   - select HTTP and SSH

![Screenshot (1102)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/4005f0a7-a43d-4a29-9eb3-68ccbf0af9a5)

- Click on next and go to networking
- Click on Create new  and create a virtual network 
- Create to subnet groups one for VM's and another for Application Gateway.

![Screenshot (1103)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/97c95695-df0d-4449-8403-f60fe247a26a)

- Change public IP address name as your wish.
- Then finally click on Review + create
![Screenshot (1104)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/437bd53f-b400-4fd4-9b66-0f0d128bc5ce)

- Same as above create another VM in same region.
- Give name as CenteralusVM-2

![Screenshot (1106)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/896fcbad-855b-4349-b39e-c87b8bb1ed46)
- Select same Virtual networking ans subnet as above.
- Then, click on Review + create

![Screenshot (1107)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/64bcc39a-e55e-4753-b90f-5c7a1ec57055)

- Now we have created 2 VM's in Central US Region.
- Now create 2 more VM's in another Region like West Us.
- Follow the above procedure and create 2 VM's 
- Give name as westusVM-1
![Screenshot (1108)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/66a23eb3-a495-4a85-a14b-bee9a3fb555a)

![Screenshot (1109)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/8b0a4cf7-4e8c-4839-8ad0-efb39dcfcc20)

- Now create new Virtual network ,subnets
![Screenshot (1110)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/47b2ac0c-e936-4bdc-8888-6b7e8e5c8dd5)
- Click on Review + create
- Now create another VM in same Region.
- Create name as westusVM-2.
![Screenshot (1112)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/5aeb9adf-d914-4157-8355-9dfeb6f09485)

![Screenshot (1113)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/f13804a5-c1c5-4612-a0fa-261861a2d11b)

![Screenshot (1114)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/ed2e960a-643a-4da9-aae4-85a3689d81fe)
- Now click on Review + create

![Screenshot (1115)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/d46c2588-f19f-4882-a1d6-195c38938b18)

# Step2 : Create Storage account:
- Search for storage account in search bar.
- Give unique name for storage account as vishu15021.
- Select any region as you wish.
![Screenshot (1125)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/3a71dcc8-498f-4dba-a313-9210cba211bf)
- Check the box as showen.

![Screenshot (1126)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/ea7b608f-4a0d-4ce2-ae5f-7735052eee36)

- Create a container with name as "uplode" 

![Screenshot (1127)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/9e1161b9-096f-470c-b02f-ab7b1c53367b)

![Screenshot (1128)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/586b136c-bd24-4c1b-8d83-23fbfba01b84)

- Select Static Website and click on Enable.
- Copy Primary endpoint  and Secondary endpoint on note pad .


![Screenshot (1129)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/386dd678-e873-4db5-9147-b84695c4d42c)
- Select container and upload error.html file.

![Screenshot (1131)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/b7a23a08-fa40-4838-99f4-246bf1880800)

- Past the Primary endpoint in browser to Check you are able to access the error.html file or not.

![Screenshot (1132)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/9dd36294-9cfc-4deb-bc7e-faef028ebab1)

- Click on Static website and give error.html under the Error Documentation Path.
![Screenshot (1133)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/1e92f63f-c1f1-4203-9b65-890faa873b32)

# Step3 : Creating Application Gateway:

- Search for Application Gateway in search bar.
- Application Gateway name as centralusAG1
- Region as Central US
- Tier as Standard V2 
- Minimun instance count : 1
- Maximun instance count : 5

![Screenshot (1117)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/c55396e4-d40b-479d-af31-59810fe00d12)
- Select Vnet and Subnet Previously created in centralUS Virtual network Region.
- Now click on Next.


![Screenshot (1118)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/5e99080d-b28d-41c7-82cb-fbc94cdd8e92)

- Now select Public IP.
- Click on Add new and give name as centralusIP1
- Click on Next.
![Screenshot (1120)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/c5c0aba5-468a-4143-952a-fdc76f855e27)

- Creating a backend pool
- Click on Add a backend pool.
![Screenshot (1121)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/937f8fa7-6b5d-4e81-8f72-45568c06dd46)
- Name as Pool1
- Target type as Virtual machine

![Screenshot (1122)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/c46b07bf-b1e7-477c-adc2-f1f77f2aea50)

- Create another pool as Pool2 and add Virtual machine in targer type.

![Screenshot (1123)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/890f28d6-8ad9-4356-be98-937c1977b4c9)
- created 2 pools.
- Click on Next

![Screenshot (1124)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/aa0c6d90-aa1b-4896-8229-6542a4f2e178)

- Now change the Configuration.
- Give Listener name as anything as your wish like name.
- Port as 80
- In Bad Gateway-502 give the Secondary endpoint link which was created in storage account.
![Screenshot (1134)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/28ee0c78-3e00-4577-86e6-11283fd6e454)


- Give backend settings name as default.
- Click on add.

![Screenshot (1135)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/28132b10-9771-4cf2-afe5-af7746290337)

- Target type : Backend pool
- Path : /upload
- Target name : uplode
- Backend settings : default
- Backend targer : pool1
- Click on Add.
![Screenshot (1136)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/f163d8ac-4597-4003-adaf-033455faac80)

- Click on Backend Target
- Target type : Backend pool
- Backend target : Pool2
- Backend settings : default

![Screenshot (1137)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/bed6b6ba-3662-4ca5-9161-619583c0e142)
- Now we can see all configuration at once.
![Screenshot (1138)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/0caed5ec-0674-431c-81f4-cc673920d440)

- Now click on Review + create
- Now finally created one Application Gateway.
- Now Follow the same procedure to to create 2 Application Gateway in West US region.
- For reference I am attaching Screenshots.
![Screenshot (1139)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/f3f52901-7163-4e19-bfce-436dab66ec3c)

![Screenshot (1140)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/9409a2ba-d111-4d1f-afe4-53006c28e1ea)

![Screenshot (1141)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/013dbb47-23c8-4d9b-b219-d61568e72a3a)


![Screenshot (1142)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/4e36be4c-bd6d-497b-846d-4fba05dd5db0)

![Screenshot (1143)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/663bef2d-112d-4b62-ac45-5d6afaf08f91)

![Screenshot (1144)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/faf999cb-d6ce-4ed1-8da7-ff8f33b552fd)

![Screenshot (1145)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/1b3696b6-6d3d-4782-a169-2b3f2b1d0833)

![Screenshot (1146)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/244d493e-19ea-47e1-b278-e45def22ab93)

![Screenshot (1147)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/2fad6197-ab63-4665-9b9b-f67eb3faa1a0)

- Finally we had created 2 Application Gateway in 2 different Regions.


![Screenshot (1148)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/b5aacbf3-3f4a-44f6-ab50-9f313e99558d)

- Now we are Creating DNS names for 2 Application Gateway.
- Now select centralusAG1 Application Gateway 

![Screenshot (1150)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/b00b9498-3c9f-47ad-86f7-470e2a860890)

- Click on configuration.
- Give DNS name lable as "agdns1" it should be unique.
![Screenshot (1151)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/6ef2af2b-a563-4be0-87b8-b310769dcbb5)

- Now we can chek DNS name in overview section of centralusAG1 in Application Gateway.
![Screenshot (1152)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/7540ee4b-00a4-45d1-bbbe-ecf9f6396062)

- Now Follow same as above and create DNS name for westusAG-2

![Screenshot (1153)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/0542dc42-7ae4-4fc6-b3f6-7ecfdb72fbee)

- DNS name lable as "agdns2"


![Screenshot (1154)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/090c0562-31ef-48ec-bd03-205b7d9ffe2f)

![Screenshot (1155)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/b26346d8-e20d-4312-8cc2-05f5a4cc2288)
# Step4 : Creating Azure Traffic Manager:

- Search for Traffic Manager in search bar.
- Click on Traffic Manager.
![Screenshot (1156)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/b3c14eb9-6a6a-4045-8b3c-e08992fb73c8)

- Give name as "azuretrafficmanagerdns"
- Routing method : Performance

![Screenshot (1157)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/f0d366c6-8dd2-4712-90a3-803ad98798b6)
- Click on create.

![Screenshot (1158)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/05f4f3d2-5daf-4ce4-8015-a678868b931e)

- Select azuretrafficmanagerdns and click on Endpoints and configure it.
- Type : Azure endpoint
- Name : endpoint1 
- Target Resource type : Public IP
- Public IP address : centralusIP1
- Click on Add and give 
- Name : endpoint2
- Public IP address : westusIP2
![Screenshot (1159)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/bd070132-c6b7-45de-a166-286a73f4bc24)

- Now we had created 2 endpoints.
![Screenshot (1160)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/cc76738c-6571-4cce-a81c-ddee0f57146f)

# Step4 : Login to VM's:

- Login to all four VM's with public ip ,user name and password
throught putty.
![Screenshot (1161)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/61ecd176-4bc2-4022-ade7-becc733607b0)

![Screenshot (1162)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/3943a7f5-3d8d-4eed-8836-c0770e391614)

![Screenshot (1163)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/687d0f50-7795-44a0-a09c-9210a36d1f8b)

- Clone the GitHub repo https://github.com/azcloudberg/azproject to all the VMs.

![Screenshot (1164)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/1eb19294-8393-4e08-8898-ae7f9dcc2137)

## Commands

Run these command in all VM's

```bash
  sudo apt-get update
  git Clone https://github.com/azcloudberg/azproject.git
  cd azproject/
  ls
```

![Screenshot (1165)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/ae883f42-4271-4edc-996e-8e10927fc069)

![Screenshot (1166)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/a77374c0-d4d1-40a1-aa9e-5366ebe9a42e)

### Commands to run on VM1:

```bash
   .vm1.sh
   sudo nano config.py 
   ctrl + S
   ctrl + x
   sudo python3 app.py

```
- After running "sudo nano config.py" we need to change:
- account = <your storage accunt name>
- key = <your storage account access key>

![Screenshot 2024-05-04 184718](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/67454a61-cbb4-414e-be87-657e87b4c7ac)
```bash
   
   sudo python3 app.py

```

![Screenshot (1167)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/a3851827-fd84-4328-a413-94177b92da88)


### Commands to run on VM2:

```bash
   .vm2.sh
  
```

- Now both endpoints are in online.
![Screenshot (1168)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/4cabbbeb-3ba9-4650-88ff-3a0cd5f20131)

- Now we can copy the Azure Traffic Manager DNS name and browser in google.

- We can access home page 


![Screenshot (1169)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/33e013e4-4148-4e59-a241-131032bc7f7f)

- By adding /upload to the Traffic Manager DNS we can get upload page.
![Screenshot (1170)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/51e94d27-cf74-4668-b0fb-95bb5a2f380b)

- Here we can see VM1_key.pem had uplode through upload page.



![Screenshot (1172)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/9c980f92-b2e6-4471-af6b-eef240a2edd9)

- Finally we can check the file or folders upload from web page in storage account from Azure console.


![Screenshot (1173)](https://github.com/KatamreddyVishnu/Deploying-Web-pages/assets/155063589/23642bac-c6ce-47ea-8bc9-e5a06ebde94c)


# Thank you!
