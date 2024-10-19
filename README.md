# System Hardening

**Overview:** This lab covered the hardening of a Linux system by implementing security controls, configuring firewalls, and managing secure protocols.

**Skills Developed:** System hardening, firewall configuration, secure FTP and SSH management, implementing security best practices.

**Tools Used:** Linux Firewall (UFW), SSH, FTP, ifconfig.

---

**Lab Details**


Introduction
Wireshark is a basic tool used for observing messages exchanged between executing protocol entities called a packet sniffer. A packet sniffer passively copies messages being sent from and received by the user’s computer. The packet information displayed includes, but is not limited to various protocol fields.
Objective
The objective of this week’s lab is to familiarize the user with the Wireshark program and observe the network protocols in his or her computer. The following tasks are to be performed:
•	Download and install Ubuntu on Virtual Box
•	Communicate between Kali Linux and Ubuntu Virtual Machines
•	Perform hardening of system
•	Analysis questions
Date and time of analysis:
•	November 7, 2022 at approximately 10:00 am 
Hardware used to perform analysis:
•	Hewlett Packard Pavilion dv7 Notebook PC
•	Intel(R) Core(TM) i5-2430 CPU @ 2.40GHz 
•	8.00 GB DDR3 RAM
•	64-bit OS
Software and Websites used to perform analysis:
•	Windows 11 Software
•	Ubuntu Linux
•	Kali Linux
•	Virtual Box 


Results and Analysis
To begin this lab, the user should first check and make sure the ubuntu virtual machine is updated to the latest version. By doing this, the user can use the ‘sudo apt-get update’ command. Once the update is finished the user needs to set another user to the ubuntu virtual machine. The user can execute the ‘sudo adduser test’ command. This will add a new user and the name of the user will be called ‘test’. The user will give a password to the new user then there will be a new user added to the Ubuntu machine. Once the additional user as been added the individual can now open the Kali Linux virtual machine to add a communicable way between both the Ubuntu and Kali Linux machines. The individual will go into the Virtual box properties on the home screen and then go to Network. The user will then click the create button the create a new network under the NAT networks. The NAT network should appear, however, its always best to make sure it’s the correct category. At the bottom of the window, it will allow the user to name the connection and what the IP address will be. In this example, the Name is Connection1 and the IP address is 192.168.99.0/20. The user should check the ‘Enable DHCP’ box as well when choosing a name and an IP address. The figure below is what the window will appear to be when allowing to make the communication between virtual boxes.
Figure 1: Virtual Box preferences

 ![image](https://github.com/user-attachments/assets/3af84d15-d798-40a5-afcb-3ea69e5cc42a)



Once the user has set the preferences in the man virtual box. The user will go into each machine and change the Network settings from NAT to NAT Network and below the NAT Network tab change the name to the name that was created by the user. In this example, it would be ‘Connection1’. If it doesn’t appear right away, close out of it and go back into the same window the same way and look for the created IP address name again. Once it is found, hit OK. Do the same steps for both machines, the Kali and Ubuntu Machines. After this is done, the user can execute the ‘ifconfig’ command and see the inet IP addresses have been changed and now when the user executes the ping command using the other virtual machines. For example, on the Kali machine pinging Ubuntu with the command ‘Ping 198.168.99.5’ the Kali machine will be communicating with the Ubuntu machine. When executing ‘Ping 198.168.99.4’ the Ubuntu machine will be communicating with the Kali machine. The figure below shows both the ‘ifconfig’ information and the ping executions. 
Figure 2: Virtual machine communication
 

![image](https://github.com/user-attachments/assets/2067b1f4-e5af-4239-8267-ced1b26f2529)


Checking the version of the ubuntu system, the user will execute the ‘sudo ufw –version’ command. This will tell the user what version of Ubuntu is running. After entering the users password the edition will appear. The edition that is being run is ufw 0.36.1. Next the user will run ‘ifconfig’ to see what the IP address is of the Ubuntu machine. The individual should have an idea of what it will be since during the setup the individual just created one for the machines to communicate between each other. In this case, the Ubuntu machine’s IP address is 192.168.99.5 as seen in the Figure below. 

Figure 3: Ubuntu version and IP address

![image](https://github.com/user-attachments/assets/c469758f-bd9b-4177-9205-873dea483260)


Now that the user has the IP address of the Ubuntu machine, the user will then run nmap in the Kali Linux machine to see what is running. In the figure below are the results from the nmap scan.

Figure 5: Nmap of Ubuntu

![image](https://github.com/user-attachments/assets/ed498a84-3e74-49dc-9d77-3628fc6badd1)

 
Next the user will need to configure the ssh file. By doing this the user will execute the ‘Sudo vi /etc/ssh/sshd_config’ command. Once the file appears, the user will need to scroll through the file and find the line that states ‘PermitRootLogin’ and change this to no. To change anything in the file the user will click the ‘I’ key to change from read only to insert. Once the user has changed the line from ‘yes’ to ‘no’. The user will hold ‘control’ and ‘C’ at the same time to exit the insert function to go back to read only. 


Figure 6: ssh configuration

![image](https://github.com/user-attachments/assets/f2d41c13-bb90-43de-9410-dc44b12b0925)

 
Next, the user will need to execute the command ‘ssh (the test user created)@(Ubuntu Inet address)’ in this example, the command is ‘ssh testuser@198.168.99.5. After the user will need to enter their password for the Kali machine. Once the password is entered, the user will run the ‘sudo cat /etc/shadow’ command. The following will display in the figure below after some time.


Figure 7: admin user ssh

 ![image](https://github.com/user-attachments/assets/5174d090-9ed8-4d20-96c0-05c0fb1c5efe)


Next the user can attempt to execute the same commands from the test user account in the Kali Linux machine. However, when running these commands even giving sudo privileges to the test user account. The results displayed in permission denied or a wrong password. Although, it was the correct password to login to the account. This did display the ‘key fingerprint’ and also results of could not create a directory of /home/testuser/.ssh. More information is displayed in the figure below.

Figure 8: test user ssh

 ![image](https://github.com/user-attachments/assets/e3de203b-ddb0-4431-a0de-8c2b393f058b)


To begin the process for file transfer protocol(FTP) the user must install the configuration package using the ‘sudo apt-get install vsftpd’ command. Once the package is downloaded the user must go into the configuration and make sure the anonymous_enable=no in the file. By doing this the user can execute the ‘sudo vi /etc/vsftpd.conf’ command to ensure the configuration is correct. Once the configuration is correct its time to use the ftp package. The user needs to make sure there is nothing running on port 21. The user can view this by running ‘sudo netstat -tulpn | grep :21’ if the output from the command returns nothing or zeros followed by ‘:21’ then the user can start the vsftpd package. To start the package the user will run the ‘sudo /etc/init.d/vsftpd start’ command. This should return a starting vsftpd service output. Now the user can run the ftp command with the IP address of the users choice. In this example, the IP address will be from the other virtual machine. The command will be ‘sudo ftp 192.168.99.5’ since that is the IP address of the other virtual machine. Once it connects, the user will enter the name of the user it wishes to transfer and the password. Once the login is successful, the user will be able to run commands. If the user runs the ‘ls’ command the user should be able to see the list of directories of the user it typed in. In this example, it would be the ubuntu machine with the IP address of 192.168.99.5. The figure below displays an example of a connection and what the directories list in the Kali Linux machine looks like of the Ubuntu machine.

Figure 9: FTP from Kali Linux

![image](https://github.com/user-attachments/assets/bbb0d0f1-c639-49a2-be56-04cd9e7ac8a7)

 

Running the ftp from a testuser without sudo privileges results in the same display only somewhat different. The code for entering extended passive mode has different numbers displayed. Below in figure 10 is what is displayed when running from testuser. 
Figure 10: FTP from testuser
 
 ![image](https://github.com/user-attachments/assets/6c398079-971e-4cc9-9514-58b88c76db06)


Next the user must run nmap from the Kali machine to see what is open now. The user should execute ‘sudo nmap (IP address of Ubuntu machine)’ In this case the command executed is ‘sudo nmap 192.168.99.5’ Below is the output from the nmap scan. 
Figure 11: Nmap scan of Ubuntu
 
![image](https://github.com/user-attachments/assets/12e8a299-56fb-4241-9631-38854a488206)


Next the user will enable an uncomplicated firewall (UFW) on the Ubuntu machine. First the user needs to install the UFW package with the ‘sudo apt-get install ufw’ command. Then enter the users password. Once the ufw package is installed, it’s going to automatically be inactive. Its good to check the status of the ufw by entering the ‘sudo ufw status’ to see the status. The return output will state ‘Status: inactive’. By enabling the ufw the user will have the execute the ‘sudo ufw enable’ command. This turns on the firewall and will return an output of ‘Firewall is active and enabled on system startup’. Next the user can stat to apply rules for the firewall. The two rules the user is going to concentrate on are denying ssh and ftp protocol. By doing this the user will separately enter the commands for each rule. For the first rule, the command will be ‘sudo ufw deny ssh’ then the second command will be ‘sudo ufw deny ftp’. The return output for both rules should state ‘rule updated, rule update (v6)’. This means the rule was updated in Ivp4 and Ivp6 protocol.  In the figure below is how the commands should present themselves.
Figure 12: ufw commands

![image](https://github.com/user-attachments/assets/05401ad6-9391-46f8-815c-888c731a847f)


 
Then after the firewall is enabled and the rules are applied. The user wants to double check that the firewall is working properly by running nmap and attempting to login again against those same rules. In the figure below is an example of testing the rules made by the ufw command in the Ubuntu machine.






Figure 13: testing the ufw
 
![image](https://github.com/user-attachments/assets/77b674f1-02fe-47e6-85f9-1f0d38758377)





Conclusion
System hardening is a concept that all cyber professionals should understand and know how to use. This can be a good skill to have when working in a large company with numerous employees or a large network with multiple sub stations around with little cyber professionals. This a introduction level to system hardening but would still work and there are other levels of system hardening. Additionally, this a skill every cyber professional needs to know if they want to start in adding levels of prevention against attacks for the system they are working on. 
