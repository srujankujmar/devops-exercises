https://medium.com/edureka/chef-tutorial-8205607f4564


Chef Tutorial - Transform Infrastructure Into Code

Chef Tutorial - Edureka
Chef is a tool used for Configuration Management which closely competes with Puppet. Chef is an automation tool that provides a way to define infrastructure as code.

In this Chef Tutorial following topics will be covered:

Chef Architecture

![image](https://user-images.githubusercontent.com/61962773/224634864-3ed98014-7e90-45b8-b5c6-6979a6eb327a.png)

Hands-on Demo
The first section of this Chef Tutorial blog will explain you the Chef architecture in detail, which will clear all your doubts.

Chef Architecture
As shown in the diagram below, there are three major Chef components:

Workstation
Server
Nodes

Chef Architecture - Chef Tutorial
Workstation
The Workstation is the location from which all of Chef configurations are managed. This machine holds all the configuration data that can later be pushed to the central Chef Server. These configurations are tested in the workstation before pushing it into the Chef Server. A workstation consists of a command-line tool called Knife, that is used to interact with the Chef Server. There can be multiple Workstations that together manage the central Chef Server.
![image](https://user-images.githubusercontent.com/61962773/224634946-1ab9b4ed-6e85-4e77-8c0d-3ad231f6ac18.png)


Workstations in Chef - Chef Tutorial
Workstations are responsible for performing the below functions:

Writing Cookbooks and Recipes that will later be pushed to the central Chef Server
Managing Nodes on the central Chef Server
Now, let us understand the above-mentioned points one by one.

Writing Cookbooks and Recipes that will later be pushed to the central Chef Server
Recipes: A Recipe is a collection of resources that describe a particular configuration or policy. It describes everything that is required to configure part of a system. The user writes Recipes that describe how Chef manages applications and utilities (such as Apache HTTP Server, MySQL, or Hadoop) and how they are to be configured.

These Recipes describe a series of resources that should be in a particular state, i.e. Packages that should be installed, services that should be running, or files that should be written.

Later in the blog, I will show you how to write a Recipe to install Apache2 package on Chef Nodes by writing a ruby code in Chef Workstation.

Cookbooks: Multiple Recipes can be grouped together to form a Cookbook. A Cookbook defines a scenario and contains everything that is required to support that scenario:

Recipes, which specifies the resources to use and the order in which they are to be applied
Attribute values
File distributions
Templates
Extensions to Chef, such as libraries, definitions, and custom resources
Managing Nodes on the central Chef Server

The Workstation system will have the required command-line utilities, to control and manage every aspect of the central Chef Server. Things, like adding a new Node to the central Chef Server, deleting a Node from the central Chef Server, modifying Node configurations etc., can all be managed from the Workstation itself.

Now let us see, what components of Workstation are required to perform the above functions.

Workstations have two major components:
Knife utility: This command line tool can be used to communicate with the central Chef Server from Workstation. Adding, removing, changing configurations of Nodes in a central Chef Server will be carried out by using this Knife utility. Using the Knife utility, Cookbooks can be uploaded to a central Chef Server and Roles, environments can also be managed. Basically, every aspect of the central Chef Server can be controlled from Workstation using Knife utility.

A local Chef repository: This is the place where every configuration component of central Chef Server is stored. This Chef repository can be synchronized with the central Chef Server (again using the knife utility itself).

Chef Server
The Chef Server acts as a hub for configuration data. The Chef Server stores Cookbooks, the policies that are applied to Nodes, and metadata that describes each registered Node that is being managed by the Chef-Client.

Nodes use the Chef-Client to ask the Chef Server for configuration details, such as Recipes, Templates, and file distributions. The Chef-Client then does as much of the configuration work as possible on the Nodes themselves (and not on the Chef Server). Each Node has a Chef Client software installed, which will pull down the configuration from the central Chef Server that is applicable to that Node. This scalable approach distributes the configuration effort throughout the organization.

Chef Nodes
Nodes can be a cloud-based virtual server or a physical server in your own data center, that is managed using central Chef Server. The main component that needs to be present on the Node is an agent that will establish communication with the central Chef Server. This is called Chef Client.

Chef Client performs the following functions:

It is responsible for interacting with the central Chef Server.
It manages the initial registration of the Node to the central Chef Server.
It pulls down Cookbooks and applies them on the Node, to configure it.
Periodic polling of the central Chef Server to fetch new configuration items if any.
Advantages of Chef:

This Chef tutorial will be incomplete if, I don’t include the key benefits of Chef:

You can automate an entire infrastructure using Chef. All tasks that were manually being done, can now be done via Chef tool.
You can configure thousands of nodes within minutes using Chef.
Chef automation works with the majority of the public cloud offerings like AWS.
Chef will not only automate things, but will also keep the systems under consistent check, and confirm that the system is in fact configured the way it is required (Chef Agent/Client does this job). If somebody makes a mistake by modifying a file, Chef will correct it.
An entire infrastructure can be recorded in the form of a Chef repository, that can be used as a blueprint to recreate the infrastructure from scratch.
Hands-On
Here I will explain to you how to create a Recipe, Cookbook, and Template in Chef Workstation. I will also explain to you how to deploy a Cookbook from Workstation to Chef-Client (Chef Node).

I am using two Virtual Images one for Chef Workstation and other for Chef Node. For Chef Server I will use the hosted version of Chef (on the cloud). You can also use a physical machine for Chef Server as well.

Step 1: Install Chef DK (Development Kit) in your Chef Workstation.

![image](https://user-images.githubusercontent.com/61962773/224635000-d6bc117c-f0d8-4dd3-b6c1-4aeb520ef9d4.png)

Chef DK is a package that contains all the development tools that you will need when coding Chef. Here is the link to download Chef DK.


Here, choose the operating system that you are using. I am using CentOS 6.8 .So, I will click on Red Hat Enterprise Linux.

![image](https://user-images.githubusercontent.com/61962773/224635038-3eac6feb-5bc4-4f5b-9ba2-32e0af4ad392.png)

Copy the link according to the version of CentOS that you are using. I am using CentOS 6, as you can see that I have highlighted in the above screenshot.

Go to your Workstation terminal and download the Chef DK by using wget command and paste the link.

Execute this:

wget https://packages.chef.io/stable/el/6/chefdk-1.0.3-1.el6.x86_64.rpm

![image](https://user-images.githubusercontent.com/61962773/224635072-7f3b2f6c-0650-4395-bb4b-5bb3079ff1b5.png)


The package is now downloaded. It is time to install this package using rpm.

Execute this:

rpm -ivh chefdk-1.0.3-1.el6.x86_64.rpm

Chef DK is now installed in my Workstation.

Step 2: Create a Recipe in the Workstation

Let us start by creating a Recipe in the Workstation and test it locally to ensure it is working. Create a folder named chef-repo. We can create our Recipes inside this folder.

Execute this:

mkdir chef-repo 
cd chef-repo
![image](https://user-images.githubusercontent.com/61962773/224635135-2f69ddd5-2dab-4cec-b8d7-d7f07519e80f.png)


In this chef-repo directory, I will create a Recipe named edureka.rb. .rb is the extension used for ruby. I will use vim editor, you can use any other editor that you want like gedit, emac, vi etc.

Execute this:

vim edureka.rb
Here add the following:

file '/etc/motd' do
content 'Welcome to Chef'
end

![image](https://user-images.githubusercontent.com/61962773/224635183-c7efcd8a-9d2e-4afc-921c-1501d3cfe4bc.png)


This Recipe edureka.rb creates a file named /etc/motd with content “Welcome to Chef”.

Now I will use this Recipe to check if it is working.

Execute this:

chef-apply edureka.rb

![image](https://user-images.githubusercontent.com/61962773/224635213-f6c0ddc2-9b9b-42da-9b68-9bb4b5ba73fa.png)

So there is a file created in the chef-repo that has content Welcome to Chef.

Step 3: Modifying the Recipe file to install httpd package

I will modify the Recipe to install httpd package on my Workstation and copy an index.html file to the default document root to confirm the installation. The default action for a package resource is installation, hence I don’t need to specify that action separately.

Execute this:

vim edureka.rb
Over here add the following:

package 'httpd'
service 'httpd' do
action [:enable, :start]
end
file '/var/www/html/index.html' do
content 'Welcome to Apache in Chef'
end

![image](https://user-images.githubusercontent.com/61962773/224635239-25859d1c-73db-4d34-b051-7da2345f4847.png)


Now I will apply these configurations by executing the below command:

Execute this:

chef-apply edureka.rb


![image](https://user-images.githubusercontent.com/61962773/224635269-5a2f42d6-09f3-4b50-83f4-e4e916a4e762.png)

The command execution clearly describes each instance in the Recipe. It installs the Apache package, enables and starts the httpd service on the Workstation. And it creates an index.html file in the default document root with the content “Welcome to Apache in Chef”.

Now confirm the installation of Apache2 by opening your web-browser. Type your public IP address or the name of your host. In my case, it is localhost.

![image](https://user-images.githubusercontent.com/61962773/224635310-47699a4c-0719-4502-9453-a1a791397a92.png)



Step 4: Now we will create our first Cookbook.

Create a directory called cookbooks, and execute the below command to generate the Cookbook.

Execute this:

mkdir cookbooks
cd cookbooks
chef generate cookbook httpd_deploy
httpd_deploy is a name given to the Cookbook. You can give any name that you want.
![image](https://user-images.githubusercontent.com/61962773/224635353-bacbf11c-7336-4942-bb62-1d91434a1ca9.png)


Let us move to this new directory httpd_deploy.

Execute this:

cd httpd_deploy
Now let us see the file structure of the created Cookbook.

Execute this:

tree

![image](https://user-images.githubusercontent.com/61962773/224635377-8d4f31e8-2605-4f54-a43a-8d49374fb504.png)

Step 5: Create a Template file.

Earlier, I created a file with some contents, but that can’t fit with my Recipes and Cookbook structures. So let us see how we can create a Template for index.html page.

Execute this:

chef generate template httpd_deploy index.html

![image](https://user-images.githubusercontent.com/61962773/224635406-ccfd829f-11f7-42aa-8280-4daf6cb785d8.png)

![image](https://user-images.githubusercontent.com/61962773/224635419-2aaa1ea4-d7df-4414-a480-8ffeec269f2a.png)



Now if you see my Cookbook file structure, there is a folder created with the name templates with index.html.erb file. I will edit this index.html.erb template file and add my Recipe to it. Refer the example below:

Go to the default directory

Execute this:

cd /root/chef-repo/cookbook/httpd_deploy/templates/default
Over here, edit the index.html.erb template by using any editor that you are comfortable with. I will use vim editor.

Execute this:

vim index.html.erb
Now add the following:

Welcome to Chef Apache Deployment
Step 6: Create a Recipe with this template.

Go to the Recipes directory.

Execute this:

cd /root/chef-repo/cookbooks/httpd_deploy/recipes
Now edit the default.rb file by using any editor that you want. I will use vim editor.

Execute this:

vim default.rb
Over here add the following:

package 'httpd'
service 'httpd' do
action [:enable, :start]
end
template '/var/www/html/index.html' do
source 'index.html.erb'
end


![image](https://user-images.githubusercontent.com/61962773/224635463-c3d5bf6c-0c13-4983-b111-ee27fe658912.png)

Now I will go back to my chef-repo folder and run/test my recipe on my Workstation.

Execute this:

cd /root/chef-repo 
chef-client --local-mode --runlist 'recipe[httpd_deploy]'

![image](https://user-images.githubusercontent.com/61962773/224635515-3f7f613b-cbcd-4167-9303-5baf1bf8a1e4.png)


According to my Recipe, Apache is installed on my Workstation, service is being started and enabled on boot. Also a template file has been created on my default document root.

Now that I have tested my Workstation. It’s time to setup the Chef Server.

Step 7: Setup Chef Server

I will use the hosted version of Chef Server on the cloud but you can use a physical machine as well. This Chef-Server is present at manage.chef.io


Over here create an account if you don’t have one. Once you have created an account, sign-in with your login credentials.

![image](https://user-images.githubusercontent.com/61962773/224635549-d7832ad0-bd5a-4f58-8d80-6b260ea35ded.png)


![image](https://user-images.githubusercontent.com/61962773/224635591-0afa92dd-b7d3-42e3-8975-036a2fa6934a.png)


This is how Chef Server looks like.

If you are signing in for the first time, the very first thing that you will be doing is creating an organization. Organization is basically a group of Machines that you will be managing with the Chef Server.

First, I will go to the administration tab. Over there, I have already created an organization called edu. So I need to download the starter kit in my Workstation. This starter kit will help you to push files from the Workstation to the Chef Server. Click on the settings icon on the right-hand side and click on Starter Kit.



![image](https://user-images.githubusercontent.com/61962773/224635627-8c41d44b-52ef-4308-a9f4-32428fb4a14f.png)

When you click over there you will get an option to download the Starter Kit. Just click on it to download the Starter Kit zip file.


![image](https://user-images.githubusercontent.com/61962773/224635660-d710bfe2-efa0-4504-a789-8d317e59ec49.png)


Move this file to your root directory. Now unzip this zip file by using unzip command in your terminal. You will notice that it includes a directory called chef-repo.

Execute this:

unzip chef-starter.zip


![image](https://user-images.githubusercontent.com/61962773/224635695-e8b7580b-d024-43e8-814e-e4d1f7feff9d.png)





Now move this starter kit to the cookbook directory in chef-repo directory.

Execute this:

mv starter /root/chef-repo/cookbook
Chef Cookbooks are available in the Cookbook Super Market, we can go to the Chef SuperMarket. Download the required Cookbooks from supermarket.chef.io. I’m downloading one of the Cookbook to install Apache from there.

Execute this:

cd chef-repo
knife cookbook site download learn_chef_httpd

![image](https://user-images.githubusercontent.com/61962773/224635739-36bbc6b5-3c78-4915-8779-a8422da8cb5a.png)


There is Tar ball downloaded for the Apache Cookbook. Now, we need to extract the contents from this downloaded Tar file. For that, I will use tar command.

tar -xvf learn_chef_httpd-0.2.0.tar.gz

https://miro.medium.com/v2/resize:fit:1100/format:webp/1*8er_lSd9Z04qohIPtYZmMA.png

All the required files are automatically created under this Cookbook. There is no need to make any modifications. Let’s check the Recipe description inside my recipes folder.

Execute this:

cd /root/chef-repo/learn_chef_httpd/recipes
cat default.rb

![image](https://user-images.githubusercontent.com/61962773/224635800-3cbec2b7-ec1d-44e6-89aa-1d5c1a318f67.png)


Now, I will just upload this cookbook to my Chef Server as it looks perfect to me.

Step 8: Upload Cookbook to the Chef Server.

In order to upload the Apache Cookbook that I have downloaded, first move this learn_chef_httpd file to the Cookbooks folder in the chef-repo. Then change your directory to cookbooks.

Execute this:

mv /root/chef-repo/learn_chef_httpd /root/chef-repo/cookbooks
Now move to this cookbooks directory.

Execute this:

cd cookbooks
Now in this directory, execute the below command to upload the Apache Cookbook:

Execute this:

knife cookbook upload learn_chef_httpd

![image](https://user-images.githubusercontent.com/61962773/224635840-8ec4b0db-9b79-41e2-aa40-b2bbe66b45cf.png)


Verify the Cookbook from the Chef Server Management console. In the policy section, you will find the Cookbook that you have uploaded. Refer the screenshot below:


![image](https://user-images.githubusercontent.com/61962773/224635889-49e1f87b-206d-4f92-bccc-dc7dede9aece.png)


Now our final step is to add Chef Node. I have setup a Workstation, a Chef Server and now I need to add my Clients to the Chef Server for automation.

Step 9: Adding Chef Node to the Chef Server.

For demonstration purpose, I will use one CentOS machine as Chef Node. There can be hundreds of Nodes connected to one Chef Server. The terminal color of my Node machine is different from the Workstation so that you will be able to differentiate between both.

I just need the IP address of my Node for that I will execute the below command in my Node machine.

Execute this:

ifconfig

![image](https://user-images.githubusercontent.com/61962773/224635919-38bfa4ed-4f1b-43a1-a986-cd1e394d5df3.png)

I will add my Chef Node to the Server by executing Knife Bootstrap command in which I will specify the IP address of The Chef Node and its name. Execute the command shown below:

Execute this:

knife bootstrap 192.168.56.102 --ssh-user root --ssh-password edureka --node-name chefNode
![image](https://user-images.githubusercontent.com/61962773/224635954-6308d1cc-db88-4bb4-baba-e658669a8876.png)

![image](https://user-images.githubusercontent.com/61962773/224635973-530193a8-6aaf-4f24-8281-57af4abf2600.png)


When you initially bootstrap your nodes, you can set the Chef Infra client to run immediately so the node is immediately configured correctly, and also set the Chef Infra client to run periodically so it can grab the latest updates to the cookbooks you uploaded to Chef Infra server.

This command will also initialize the installation of the Chef-Client in the Chef Node. You can verify it from the CLI on the Workstation using the knife command, as shown below:

Execute this:

Knife node list

![image](https://user-images.githubusercontent.com/61962773/224635995-d8ff4c11-a6f2-4ec6-b54a-b892a52bee97.png)


You can also verify from the Chef Server. Go to the nodes tab in your Server Management Console, here you will notice that the node that you have added is present. Refer the screenshot below.
![image](https://user-images.githubusercontent.com/61962773/224636011-b0ac28a7-9fe7-40c0-826a-77df3ca850fc.png)


Step 10: Manage Node Run List

Let’s see how we can add a Cookbook to the Node and manage its Run list from the Chef Server. As you can see in the screenshot below, click the Actions tab and select the Edit Run list option to manage the Run list.

![image](https://user-images.githubusercontent.com/61962773/224636033-27378c59-48cb-4af4-9920-3c3563e2ccbe.png)

In the Available Recipes, you can see our learn_chef_httpd Recipe, you can drag that from the available packages to the current Run List and save the Run list.

![image](https://user-images.githubusercontent.com/61962773/224636054-f547bd74-6eac-4f2b-8070-e0c4bca05c5c.png)
![image](https://user-images.githubusercontent.com/61962773/224636066-4a55b093-f87b-4f58-b8df-0126f1244123.png)


Now login to your Node and just run chef-client to execute the Run List.

Execute this:

chef-client
![image](https://user-images.githubusercontent.com/61962773/224636097-fbc4af4d-3285-45d4-962a-1880fa501cfc.png)


I hope you enjoyed this Chef Tutorial and learned how Chef can be used to configure hundreds of Nodes. Chef is playing a vital role in many organizations to achieve DevOps. With Chef organizations are releasing applications more frequently and reliably.

So that brings an end to this blog on Chef Tutorial. If you wish to check out more articles on the market’s most trending technologies like Artificial Intelligence, Python, Ethical Hacking, then you can refer to Edureka’s official site.
