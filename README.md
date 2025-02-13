# CSC2510-001-FinalProject
Project for final 

Creating the Server Environment Setup:

1. In a private window in the web browser, log in to the GCP Cloud Console: https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwihkZvKj_eCAxVekyYFHZt9CBgQjBB6BAgPEAE&url=https%3A%2F%2Fconsole.cloud.google.com%2F&usg=AOvVaw1GxwHR1WZnDu0xsR-djCrv&opi=89978449

2. Begin by clicking on the "Create Instance" button. Repeat these steps as needed.
   - Creating the Ansible Management(AM) Servers for each environment:
       1. In the "Name" box, enter the name of your AM. Ex: am-01(test/prod/dev)
       2. In the "Region" box, you may select any of the places listed. Just be sure to keep your servers in different areas to make sure that if something does happen to one of the physical servers, it will not affect all of your servers. 
       3. "Machine Configuration" should be set to E2. Scroll down to the "Machine Type" section, and, on the drop down menu, select "e2-small".
       4. Next, scroll down to the "Boot Disk" section, and hit the "change button". In the "Operating System" box, click on the arrow and select "CentOS" followed by the "Version" box changed to "CentOS 7".
       5. Lastly, hit the "Select" button at the bottom. Then, click on "Create".
          
   - Creating the Web Servers (WS) for each environment:
       1. In the "Name" box, enter the name of your WS. Ex: ws-01(test/prod/dev)
       2. In the "Region" box, you may select any of the places listed. Just be sure to keep your servers in different areas to make sure that if something does happen to one of the physical servers, it will not affect all of your servers. 
       3. "Machine Configuration" should be set to E2. Scroll down to the "Machine Type" section, and, on the drop down menu, select "e2-small".
       4. Next, scroll down to the "Boot Disk" section, and make sure that the "Boot Disk Image" is set to "Debian GNU/Linux".
       5. Lastly, in the "Firewall" section, check the boxes for "Allow HTTP traffic" and "Allow HTTPS traffic". Then, click on "Create".
          
   - Creating the Database Servers (DBS) for each environment:
       1. In the "Name" box, enter the name of your DBS. Ex: dbs-01(test/prod/dev)
       2. In the "Region" box, you may select any of the places listed. Just be sure to keep your servers in different areas to make sure that if something does happen to one of the physical servers, it will not affect all of your servers. 
       3. "Machine Configuration" should be set to E2. Scroll down to the "Machine Type" section, and, on the drop down menu, select "e2-small".
       4. Next, scroll down to the "Boot Disk" section, and make sure that the "Boot Disk Image" is set to "Debian GNU/Linux".
       5. Lastly, in the "Firewall" section, check the boxes for "Allow HTTP traffic" and "Allow HTTPS traffic". Then, click on "Create".

3. After each environment has been created, the servers should be running. We know that they are running if there is a green check mark on the left under "Status". If there is not a green check mark, select the server(s) and click on the "Start/Resume" button at the top or by clicking on the 3 dots on the right. Once the servers have been started, locate your Ansible Management Servers (AM), and next to SSH, click on the arrow, then click on "Open in browser window". Once it opens a new window and loads, a pop-up window will appear stating "Allow SSH-in-browser to connect to VMs", hit "Authorize".
  
4. (Keep in mind that you will have to do this for the other AMs in the different environments.) Once you are in the AM server, we are going to adjust a couple of settings to ensure that the AM is connected to the WS and the DBS. Repeat this step as needed in the AMs:
   (Additionally, keep in mind that the system we are in is CentOS, which uses keyword "yum" to manage and install packages.)
   - Installing Nano:
       1. In the command line, type in "sudo yum install nano" and hit enter.
       2. It will begin to load, then it will ask "Is this ok [y/d/N]: " to which you will type in "y" and enter. Once Nano has installed, it will return "Complete".
       
   - Installing Ansible:
      1. In the command line, type in "sudo yum install ansible" and hit enter.
      2. It will begin to load, then it will ask "Is this ok [y/d/N]: " to which you will type in "y" and enter. Once Ansible has installed, it will return "Complete".

5. Next, we will change the user root password in each of your environments. This is so that we can easily access and connect our VMs from our Ansible Management Server. Repeat this step in all of your servers.
    - In the command line, type in "sudo passwd" and enter. It will ask for "New Password: ". When you are typing in your password, it will not be displayed on the screen. So, be sure that you mentally keep up with what you are typing in as your password. You will need to remember this to connect the VMs:
      <img width="615" alt="Screenshot 2023-12-05 at 10 04 29 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/ae321e5d-40a0-494a-a19c-4d13ba8860e2">


6. Next will be adjusting the Ansible Hosts file:
   - First, we need to change our directory(cd) into the hidden directory "etc". We will do this by typing into the command line "cd /etc" and hitting enter. Then, typing in "ls" and enter, which will then show the list of all of the directories in the "etc" directory. In this case, we will be looking for the directory "ansible":
    <img width="615" alt="Screenshot 2023-12-05 at 10 07 02 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/df8191f9-58c4-471d-bb12-f6111efe6a19">


    - Once we locate the "ansible" directory, we will change to that directory by typing in the command line "cd ansible" and enter. We will type in "ls" to see the files. In this case, we want to locate the hosts file:
    <img width="615" alt="Screenshot 2023-12-05 at 10 08 22 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/ea0710ab-a9d3-4dfd-bcd0-fde1e83e0ce4">


    - To access and edit this file, type in the command line "sudo nano hosts" and enter. Now, we need to add our WS and DBS to our hosts file. Throughout the hosts file are example of how to add in different hosts, or servers. Before we can connect our AM to our servers, we need to have written down the Internal IP address of the servers we want to connect to our AM. Once we have the Internal IP address, we can proceed.
    - Scrolling to the end of the hosts file, type in each of your groups with your Internal IP address as well as your root user and password, the IP and user in the screenshot are an example only(beware of the spaces):
    <img width="613" alt="Screenshot 2023-12-05 at 10 13 13 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/aee7b94c-8a51-4312-8b9a-7e83babea46d">


    - Now that we have added our servers into the hosts file and added in our root user and password, we can use the keys "Control ^ + X", then "Y" for yes, and hit enter. This will save our edits into the hosts file, now connecting our Web Servers(WS) and Database Servers(DBS) to our Ansible Management Server(AM).
    - Once you have completed these steps, lets clear out the terminal by typing in the command line "clear" and enter.
      
      HURRAY!! You have made it this far!!! The fun doesn't stop here! Keep going!!!

7. Next, we will adjust the root login and hosts permissions for all of our VMs through our SSH. Repeat these steps as needed:
  - In the command line, type in "sudo nano /etc/ssh/sshd_config" and hit enter.
  - We need to locate 2 specific options in this file. First is "PermitRootLogin" needs to be changed from "no" to "yes". Second, the first "PasswordAuthentication yes" needs to be uncommented while the second "PasswordAuthentication no" needs to be commented out(#). Both options should end up looking like this:
	<img width="616" alt="Screenshot 2023-12-05 at 10 17 22 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/a1493613-149e-47da-83ac-32e8d1dff3c4">

	<img width="616" alt="Screenshot 2023-12-05 at 10 18 17 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/702fea51-ff88-427a-be0a-a6e015d82ae5">


  - Once you have made the changes, we can use the keys "Control ^ + X", then "Y" for yes, and hit enter to save the changes into the sshd_config file.

-----------------------------------------------------------------------------------------------

Use the Ansible Playbooks to install packages(Apache, NodeJS, MariaDB) and to automate the deployment of a web application from a Git Repository through a shell script for each environment. 
Before you begin, make a new directory by typing in the command line "mkdir ansible-playbooks". Then, "cd ansible-playbooks". Ensure that you have downloaded the attached files into the new directory:
	<img width="575" alt="Screenshot 2023-12-05 at 11 46 40 PM" src="https://github.com/tpflores09/CSC2510-001-FinalProject/assets/142537354/4e6a4de8-ef2f-4679-aa3b-dbe77873282f">



- Running the "specific-environment" Bash Script:
	1. Make the script executable by running chmod 755 specific-environments.sh.
	2. Execute the script by running ./specific-environments.sh.
	3. Follow the prompts to enter the necessary configuration parameters for your environment.

- Configuring Cron Jobs:
	1. Open the crontab for editing by running crontab -e.
	2. Add cron job entries for your tasks. For example:
	3. To run a script every minute: * * * * * /home/teperdomof42/ansible_playbooks/cronjob.sh
	4. To update packages daily at midnight: 0 0 * * * ansible-playbook /home/teperdomof42/ansible_playbooks/updates.yml
	5.Save and exit the crontab editor.

- Updating the Server Environment:
	1. Navigate to the directory containing your Ansible playbooks, or “ansible-playbooks”.
	2. Run the Ansible playbook for the appropriate environment (dev,prod,test) by executing ansible-playbook ____-playbook.yml.

- Configuring the Git Repository:
	1. Clone the repository to your local machine by first making the script executable by running chmod 755 clonescript.sh. Then, by running ./clonescript.sh.
	2. Configure the remote repository for each environment by setting the appropriate branch.
	3. Push updates to the repository as needed.


----------------------------------------------------------------------------------------------------------------


Troubleshooting Steps and Common Issues:
- Script fails to execute: Ensure the script is executable and the shebang line is correct.
- Cron job not running: Check the cron syntax and ensure the cron is running.
- Ansible playbook errors: Verify the playbook syntax, check for typos, and ensure all required roles and variables are defined.
- Git repository issues: Check for connectivity to the Git server, ensure SSH keys are set up correctly, and verify branch names.
