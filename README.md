<p align="center">
<h1>Setting up a Home Lab running Active Directory (Oracle VirtualBox) and Adding users with Powershell</h1>


<h2>Overview</h2>

- Download Oracle Virtual Box. This will be used to run my virtual machines
  
- Download Windows 10 ISO and Server 2019 ISO. This will be used to install the two operating systems on two separate virtual machines
  
- Create the first virtual machine which will be a domain controller. Once everything is downaloded, I will create the first VM which will be the domain controller and It will house an active directory. This will have two network adapters. One will connect to the outside internet, the other will connect to the virtualbox private network which the clients will connect to.
  
- Assign IP addressing for the internal network. I will Install server 2019 ISO once the virtual machine is created and I will assign IP addressing for the internal network. The external network will automatically get IP addressing from my home network.
  
- Name the server. Once I have IP addressing setup, I'm going to install active directory and create my domain. I will then configure my NAT and routing so the clients in the private network can reach the internet from the domain controller.
  
- Setting up DHCP. I will then setup a DHCP on the domain controller so when I create my Windows 10 machine it can automatically  get an IP address. The main purpose of DHCP is to allow computers on a network to automatically get their IP addresses.
  
- Running Powershell Script. The last thing I will do on the domain controller is to run a powershell script that will automatically create a 1000s users on active directory.
  
- Installing Windows 10. After creating the users, I will create another virtual machine and install windows 10 which will be connected to the private virtualbox network. I'm going to name this machine client1 and join it to the domain and I will login with a domain account.  

<h2>Tools and Utilities Used</h2>

- <b>VirtualBox<b>
  
- <b>Active Directory Domain Services</b>

- <b>Windows 10 ISO (will connect to the vmware network)</b>

- <b>RAS/NAT<b>

- <b>PowerShell<b>

- <b> Server 2019 ISO (used for domain controller will house active directory)<b>

- <b>2x Network Adapters (Internet + Internal VMware Network)<b>

- <b>DHCP<b>

- <b>PowerShell Script<b>

- <b>Macbook Pro<b>

<h2>Lab walk-through:</h2>

<p align="center">
1. Setting up VM. <br/>
<br>Once I’ve downloaded VirtualBox Windows 10 ISO and Server 2019 ISO, I’ve created my first VM on virtualbox which will be the domain controller. I’ve configured the VM with 2GB of RAM and changed the shared clipboard and Drag’n’Drop stetting to bidrectional. This would allow me to copy, paste and drag filed between my desktop and virtual machine <br>
  

<img src="https://i.imgur.com/1Ups6Ys.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
1.1. Creating NICs. <br/>
<br> Because im configuring a domain controller, I will need two Network Adapters. One connected to the internet which will be running NAT and other dedicated to the internal vmware network. The first adapter is already preconfigured.<br>
  

<img src="https://i.imgur.com/DsKfqNU.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
1.2 Server 2019 ISO. <br/>
<br> 1.2. Once I’ve configured the DC Virtual Machine settings, I’ve go it up and running using server 2019 ISO. I will use basic credentials for my administration access.<br>
  

<img src="https://i.imgur.com/L6JLQxQ.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
2. Server Startup. <br/>
<br>The first thing I’ve done when my server started up was to insert Guest Additions CD Image. This helps improve the performance and usability of my VM. Once the guest addition was installed I proceeded to restart the VM.<br>
  

<img src="https://i.imgur.com/tQbG3W1.jpeg" alt="Home Lab Running Active Directory"/>


<p align="center">
2.1 Guest addition usability adjustment. <br/>
<br>Logging back in the VM When logging back in, the first notice is the mouse won’t be lagging and the screen is able to be resize with the resolution being auto adjusted. This is what the guest addition purpose is as it improves usability of virtual machines.<br>
  

<img src="https://i.imgur.com/5HNNUOD.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
3. Setting up IP addressing. <br/>
<br> Just like how I explained in my overview, I will have two network interface controllers. One dedicated to the internet and the other thats going to be used for my internal network. For the internal NIC, I will have to setup manually. I will first rename the NICS according to their purposes. <br>
  

<img src="https://i.imgur.com/N8xl2h3.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
4. Setting IP address for internal network. <br/>
<br>I’ve assigned an internal IP address without using a default gateway. This is because the domain controller itself is going to serve as the default gateaway as it has two NICs, one on the internet and one internal. For the DNS server, when I install active directory it will automatically install DNS. Therefore, this server will actually use itself as the DNS server. I used the loopback address 127.0.0.1 which refers to itself. This essentially means that whenever a computer pings the loopback address it just pings itself automatically.<br>


  

<img src="https://i.imgur.com/g1L7d4x.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
5.Renaming. <br/>
<br> I also renamed the PC to DC (for Domain Controller) just to change the arbitrary name to something more formal. The PC will restart again after this action. (There will be a lot of restarts throughout this lab project)<br>


  

<img src="https://i.imgur.com/PhB9AHZ.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
6.Install Active Directory Domain Services. <br/>
<br> Now that I have my NICs and configured the IP address of my internal network I will now install AD DS and create a domain.<br>


  

<img src="https://i.imgur.com/N8xl2h3.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
6.1. AD Deployment Configuration. <br/>
<br>Now the role has been installed, I will now have to actually create and configure the domain. Deployment operation will be ‘Add a new forest’ and for this lab project i’ve named the domain mydomain.com<br>


  

<img src="https://i.imgur.com/dWilRKh.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
6.2. Installing Active Domain. <br/>
<br>Once I’ve named the domain there is no other configurations needed and the AD will be ready to be installed. After installation, it would automatically restart the VM.<br>


  

<img src="https://i.imgur.com/2AMdXqI.jpeg" alt="Home Lab Running Active Directory"/>

<p align="center">
7. Creating Domain Admin Account. <br/>
<br>After the VM restart and logging back in I will be creating my own dedicated domain admin account. To do this I went from startup>ADD Users and Computers>mydomain.com>New Organisational Unit. Inside OU I created a folder and named it ADMINS, this is where I will be able to create administrative users. For this project I just used my name.<br>


  

<img src="https://i.imgur.com/N0QPE4w.jpeg"/>

<p align="center">
7.1 Creating Domain Admin. <br/>
<br>Even though I’ve created the account I will still need to make it a domain admin. To do that I went from the properties of the user I just created > Member Of>Add and from here I entered the object name as domain admins which now creates a domain admin account.<br>


  

<img src="https://i.imgur.com/KoCmFjj.jpeg"/>

<p align="center">
7.2 Signing In as Admin. <br/>
<br>To use the admin account I’ve signed out from the computers and use my admin credentials to login<br>


  

<img src="https://i.imgur.com/PlPRxAU.jpeg"/>

<p align="center">
8. Installing RAS/NAT. <br/>
<br>My next step is to install Network Access Translation/ Remote Access server. The purpose of this is its going to allow my windows 10 client (after I create windows 10) to have access to the internet through the domain controller whilst being in a virtual private network. To do this, on the server manager dashboard: Roles and features>server roles>remote access>roles services>routing>install.<br>


  

<img src="https://i.imgur.com/ChCXWQT.jpeg"/>

<p align="center">
8.1 Routing and Remote Access. <br/>
<br> After installing I will configure and enable routing and remote access as well as install NAT to allow internal clients to connect to the internet using an IP address.
<br>


  

<img src="https://i.imgur.com/inZwOzQ.jpeg"/>

<p align="center">
8.2 NAT Internet Connection. <br/>
<br>I will use the network adapter I’ve named “INTERNET” to be used as a public interface to connect to the internet. <br>


  

<img src="https://i.imgur.com/efVtNbj.jpeg"/>

<p align="center">
8.2 Windows 10 Client <br/>
<br>I have now completed configuring RAS and NAT which means when I have windows 10 clients they will be able to gain access to the internet after I setup the DHCP server on the domain controller. <br>


  

<img src="https://i.imgur.com/mHv7cbI.jpeg"/>

<p align="center">
9. Setting up DHCP. <br/>
<br>Setting up my DHCP will be similar to the setup steps I did for my AD DS. Only now in server roles I select DHCP.<br>


  

<img src="https://i.imgur.com/nlG3uYC.jpeg"/>

<p align="center">
9.1 Configuring DHCP. <br/>
<br>For configuring my DHCP, Im going to create a scope that will give IP address from a range of 172.16.0.100-200 with a subnet mask of 255.255.255.0.<br>


  

<img src="https://i.imgur.com/nlG3uYC.jpeg"/>


<p align="center">
9.1 Configuring DHCP. <br/>
<br>For configuring my DHCP, Im going to create a scope that will give IP address from a range of 172.16.0.100-200 with a subnet mask of 255.255.255.0.<br>


  

<img src="https://i.imgur.com/OqwlX5P.jpeg"/>

<p align="center">
9.2 DHCP Options. <br/>
<br>I will select yes to configure DHCP options. This essentially means that I want to tell the clients which server to use for DNS or gateway. This is necessary as I want the clients to be able to access the internet<br>


  

<img src="https://i.imgur.com/X3yEaUG.jpeg"/>
<p align="center">
9.3 Router (Default Gateway).  <br/>
<br>Since I’ve configured NAT for the domain controller and the domain controller has routing configurative. It’s job will be to forward traffic from clients to the internet. Because of this, clients will use the internal NIC of the domain controller as for their default gateway/router.<br>


  

<img src="https://i.imgur.com/XVH4WPZ.jpeg"/>

<p align="center">
10. Browsing.  <br/>
<br>My next step is to make a configuration that will allow browsing from the domain controller. I would first turn IE Enhanced Security Configuration off. This just stops confirmation security checks before loading every page on the internet. In a normal environment this would usually be left on but since im demonstrating on a lab it will be okay to turn it off<br>


  

<img src="https://i.imgur.com/qNjTAYA.jpeg"/>


<p align="center">
10.1. Creating users with Powershell.  <br/>
<br>My next step will be  using a source code that has the powershell script which will be used to create the users. <br>


  

<img src="https://i.imgur.com/fjm4GUl.jpeg"/>

<p align="center">
10.2. Names.<br/>
<br>I’ve opened the names file which has a bunch of names that was made through a name generator. At the top of the list I placed my own name. Im going to use this file to programatically create all of these users and for myself.<br>




  

<img src="https://i.imgur.com/We0D6X2.jpeg"/>

<p align="center">
10.3 Running Powershell Script. <br/>
<br> I’ve opened the script on Windows PowerShell ISE. Before running the script, I will have to enable the exceution of all scripts on this server otherwise I will get a security error stating the script is not digitally signed. I would do this by running Set-ExecutionPolicy Unrestricted.<br>




  

<img src="https://i.imgur.com/f1WfomI.jpeg"/>


<p align="center">
10.4 Explaining Script. <br/>
<br>Line 2-3 are variables I will be using. Line 2 contains the password that all of the user accounts will be using which is “Password1”

Line 3: Get-content .\names.txt is a script to get all the names from the txt file pasting it in the USER_FIRST_LAST_LIST array

Line 6: This variable is used to takes the plaintext password (Password1) and creates it into a object that powershell can use for a secure password ($password)

Line 7: This creates an organisational unit. I done this manually in step 7 when I was creating a domain admin account. The only difference is this will created an organisational unit called USERS. 

line 9-23:  The “foreach” is essentially a loop for the block of code to run for each individual on the USER_FIRST_LAST_LIST. The “$n” is a representation of the user thats being examined e.g the first “n” will be myself as I put my name on top of the name.txt file.

Line 10: $n.Split - What this does is it will take a n value from the name list and will split it according to the space between the first and last name  (“  “) and its going to take the first element which will be element [0] (first name) which will then store it in the $first variable 

Line 11: This is the same as line 10 but with the last name and  the last name being called element 1 “[1]".

Line 12. This line cantotonates the first name and the substring(0,1). The zero represents the firstname and the 1 tells the script to take the first character of the first name and it will glue the first character to the last name with the “$last” command. The username at the end will look like “mkamotho” in representation of Mark Kamotho.

Line 13: This is just outputting something on the screen e.g “Creating User” will just alert me that a user has been created     cyan font background black colour.

Line 15: This line creates a new user on Active Directory with the “Password1” password. The GivenName and Surname are the $first and $last objects which was created in line 10&11. The displayname, name and employeeID will all use the $username which will be the first initial and full last name
For line 21 I put PasswordNeverExpires $true. This is because this is only a lab environment for an actual productive environment stricter password requirements would’ve been used 
For ou=USERS on line 22 simply means it will store the created users under USERS in the organisational unit.
For “Enabled $true” simply means the user account is goint to be enabled<br>




  

<img src="https://i.imgur.com/f1WfomI.jpeg"/>


<p align="center">
10.5. Directory.  <br/>
<br>Before running it I would need to go to the directory where the script is located just for it to work and pull in the names.txt<br>




  

<img src="https://i.imgur.com/o3Gn90v.jpeg"/>

<p align="center">
10.6. Running the script.  <br/>
<br>I’ve now ran the script which will take a while to complete as there are a lot of names in the txt file. Although for now I can go to the AD Users and compliance and see the new Users OU be created that contains the users created through the script.<br>




  

<img src="https://i.imgur.com/YbMCzkV.jpeg"/>

<p align="center">
11. Windows 10 Client.   <br/>
<br>Now that I have my NICs, Domain, NAT and DHCP setup my last task is to create a windows 10 virtual machine in virtualbox. This will use an internal NIC which should give it’s IP address from the DHCP server that has already been configured.<br>




  

<img src="https://i.imgur.com/jGrMwPu.jpeg"/>


<p align="center">
11.1 Windows10 VM.   <br/>
<br>The main configuration I’ve done for this VM is I used the internal network network adapter. This is because I can emulate a corporate network and use the domain controller to get a DHCP address.<br>




  

<img src="https://i.imgur.com/Y0gUCsf.jpeg"/>

<p align="center">
11.2 Disk Installation.<br/>
<br>Once the VM has been configured I will install the WIndows 10 disk into the VM. This file was downloaded into my Macbook desktop from the beginning of this lab project<br>




  

<img src="https://i.imgur.com/ll5PfJN.jpeg"/>

<p align="center">
11.3 Checking internet and IP.<br/>
<br>First thing I did once I had my Windows 10 setup is to check if the internet and the IP address is working which it does as the right IP is displayed alongside the created domain.<br>




  

<img src="https://i.imgur.com/5llmC4a.jpeg"/>

<p align="center">
11.3 Ping.<br/>
<br>I pinged Google just test if my DNS server is working which it is as Google resolved. This means I can ping to the internet giving me assurance that my whole network infrastructure is working. I have connetivity to the default gateway (Domain Controller) and then from the domain controller it is forwarding it out to the internet and the ping comes back to my VM as an echo reply.<br>




  

<img src="https://i.imgur.com/GUPrJ2c.jpeg"/>
<p align="center">
11.3. Rename this PC(Advanced).<br/>
<br> I’ve renamed the PC to CLIENT1 which is accorded to my network architecture and I’ve also joined the domain that i’ve created (mydomain.com). I used my user login for the DC to join mydomain.com. I could’ve also used the admin login too it doesn’t really matter which one being used.
<br>




  

<img src="https://i.imgur.com/HXaWPfL.jpeg"/>
<p align="center">
12. Address Leases.<br/>
<br>  Back on my domain controller, I went back on the DHCP addressing leases where I can see that I now have one lease from my CLIENT computer. This ocurred when I created the client computer and joined it to the network It automatically reached to the DHCP server to request an address which the DHCP did provide.
<br>




  

<img src="https://i.imgur.com/HXaWPfL.jpeg"/>
<p align="center">
12.1. AD Users and Compliance.<br/>
<br>  Going back to my AD, I can see that after joining my CLIENT computer to the domain, the CLIENT computer can be found in my Active directory meaning that the computer is in the domain. If were to delete the CLIENT computer, I wouldn’t be able to log back in with the accounts I’ve created
<br>




  

<img src="https://i.imgur.com/9dI6NM8.jpeg"/>

<p align="center">
13. Logging to domain with the client computer.<br/>
<br>I’ve Logged back in on the domain but with the client computer using the username and password created through the powershell script. For this example I just used my name but I can use all the usernames created with the same password “Password1”(as I set the password in the powershell script the same for each username). When I logged back in I’ve ran a whoami command which given me information that I am a member of mydomain as well as my username. 
<br>




  

<img src="https://i.imgur.com/aCqbxQe.jpeg"/>

<p align="center">
14.Conclusion.<br/>
<br>Looking back to my network architecure, I’ve created a mini corporate network essentially. With the account process, it almost simulates the onboarding process when someone gets hired into a new job role. For example, hired persons name would enter a batch file and the next day the batch file script ran and created the new hiree account which can be logged in with the corporate credentials as its already in the domain.  
<br>



