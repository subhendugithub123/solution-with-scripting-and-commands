# solution-with-scripting-and-commands# solution-with-scripting-and-commands
Problem1 : Troubleshoot a server that can't connect to the internet:
# Solution : step1>1st we have to ping to any public ip like google dns ip
          $ ping 8.8.8.8
if we get the proper reply thats mean internet is working properly but if we are not getting the proper reply thats mean internet is not working properly then we can go for some troubleshooting process.
# step2>Check our network configuration using
          $ ip addr
                     or
                $ ip link   
The output of the command will show the status of each network interface on the server with “state UP” or “state DOWN”.Turn on any disabled interfaces with the next command.
         
      $ nmcli connection up <interface name>
                       or
                 $ sudo if up <interface name>
 If either of these commands fails we can go for force parameter
          
          $ sudo ifdown --force <interface name>
                 $ sudo ifup <interface name>
                  
if the problem is still persist
                  
# step3>Check the network configuration file 
To make changes to the network configuration, i will need to open the right file in a text editor.

         $ sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
The configuration file for eth0 should look like this-
                 
                   DEVICE=eth0
                   BOOTPROTO=dhcp
                   ONBOOT=yes

# Explanation the purpose of the /etc/hosts and how it is used:
# solution-
The /etc/hosts file is a plain text file used in matching a fully qualified domain name (FQDN) with the server IP hosting a specific domain.Before DNS became a network standard, the /etc/hosts file was used to resolve an IP address to a fully qualified domain name (FQDN). It’s useful if a DNS server is not available when a user wants to access a domain from their browser. When the DNS server cannot be reached, Linux uses the /etc/hosts file to resolve the domain name. 
it is used with this process. The /etc/hosts file is a plain text file used in matching an FQDN with the server IP hosting a specific domain.
The /etc/hosts file stores plain text content, but it requires a specific format. The format is:

                    <IP_address> <fully_qualified_domain_name
        # My example host file entries

                     $ 184.233.11.1 mycar.mydomain.com

                     $ 184.233.11.2 mydomain.com

                     $ 184.233.11.3 production-db.mydomain.com

#Problem2 : Install a new package on a Debian based system:
# solution-
we can install applications different ways. Terminal, the Ubuntu Software Center, and Synaptic. we can install packages via terminal.
The best part about Ubuntu and any flavor of Linux is that it comes with its own repository. A repository is basically like a store filled with thousands of packages or software. However, all the software available in the repository is open source and for Linux.

we can, of course, search the repository for available packages using the apt command. To search the repository in Ubuntu:
Installing from terminal can be done in several ways:

For example, suppose that I’m looking for a package called MySQL:

             $ sudo apt-cache search MySQL

Suppose that we’ve found the package we want but are looking for more information about the found package, then we use the apt show command.

            $ apt  show mysql-client-8.0

Next, we can check for the dependencies using the following code:
             
             $ apt  depends mysql-client-8.0

To install using the repository in Ubuntu:
             
             $ sudo apt-get install mysql-client-8.0 -y
             
Once installed, it is always a possibility that we might not like the package and wish to completely remove it from our system. To remove an installed package, type:

          sudo apt-get remove mysql-client-8.0
Apt -get remove will not remove the configuration files of the program we installed, and in those cases, we may use purge instead. To remove everything, including configuration files, we would type:

             $  apt purge mysql-client-8.0        

# configure a service to start automatically at boot time:
1. List Services which are Started Automatically
If you want to find which services are added into Systemd autostart, use the list-unit-files command with filtering by state:

            # systemctl list-unit-files --type=service --state=enabled
2. Check Service Autostart State
You can use the is-enabled command to check whether the service is already added to autostart. For example:

            $ sudo systemctl is-enabled nginx
You can use the status command instead of is-enabled. In addition, it displays whether the service is running at the moment and its latest logs:

            $ sudo systemctl status nginx           

The state can be enabled, disabled, or static. Services marked as static are added to autostart and can't be removed.
3. Enable Autostart for Service
Use the enable command to make the service start at the system startup:
 For example, run the following command to configure autostart for Nginx web-server:

            $ sudo systemctl enable nginx
When the service is not running now and you want to start it now, you can combine the enable command with the --now option. For example:

            $ sudo systemctl enable --now nginx

# Problem3 : Backup and Recovery      

# Solution 

step1> Prepare a Backup Device
To make a full backup of the system, you need a device that has much space to keep all files. A backup device can be a locally attached drive, network device, or cloud storage like Amazon S3, Azure Spaces, etc.
Create a directory to store the backup on the backup device. Assuming you have attached a separate disk in your local machine mounted at /mnt directory.

           $ mkdir /mmnt/full-backup 
           
step2> Install Rsync Utility
Rsync is a command line utility that helps to synchronize content between two directories. They either exist on the local system or one can be the remote. You can quickly install on using the default package manager on most modern Linux distributions. To install Rsync on Debian-based systems, type:
 
          $sudo apt install rsync 
step3>Automate the Backup
It’s good practice to schedule automatic backups. You can simply add the above command in crontab, or write them in a shell script and then schedule the script.

        #!/usr/bin/evn bash
 
        BACKUP_PATH="/mnt/full-backup"
        EXCLUDE_DIR='{"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"}'SOURCE_DIR="/"sudo rsync -aAXv ${SOURCE_DIR} --exclude=${EXCLUDE_DIR}  
        ${BACKUP_PATH}
 
      if [ $? -eq 0 ]; then
           echo "Backup completed successfully"
      else
          echo "Some error occurred during backup"
       fi

To schedule the script, edit the crontab:

      $ crontab -e       

Append the following entry. Make sure to set the correct script name and path. The below schedule will run the script at 02 AM every day.

      0  2  *  *  *  bash backup.sh >> backup.log
# Problem4 : System/Server 

step1> check the logs . For instance, if you have an Apache server running on an Ubuntu server, by default the logs will be kept in /var/log/apache2.
step2> check Web Server Installed:
          $ sudo apt-get update
          $ sudo apt-get install apache2

          or, 
If you are running Ubuntu or Debian and want the Nginx web server, you can instead type:

          $ sudo apt-get update
          $ sudo apt-get install nginx
          
step3> check Web Server Running:  
 ou can then grep the output of netstat for the name of the process you are looking for:         
          $ sudo netstat -plunt | grep nginx
  
   For instance, you could start the nginx service by typing:

          $ sudo systemctl start nginx      
step4> check the Syntax of your Web Server Configuration File Correct

       
          $ cd /etc/apache2
If you are using Apache, you can use the apache2ctl or apachectl command to check your configuration files for syntax errors:
        
         $ apache2ctl configtes
If you have an Nginx web server, you can run a similar test by typing:
        
         $ sudo nginx -t    
         Output
         nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
         nginx: configuration file /etc/nginx/nginx.conf test is successful

step5> check the Ports you Configured Open
You’ll need to use your remote server’s IP address and tell it what port to check, like this:
         
          $ nc -z 111.111.111.111 80

step6>Are your DNS Settings Directing you to the Correct Place?
      You can query for your domain’s “A” record by using the host command on a local or:
      
         $ host -t A example.com
         
step7>Make sure your Configuration Files Also Handle your Domain Correctly
In Apache, your virtual host file might look like this:
     
      /etc/apache2/sites-enabled/000-default.conf
      
      &lt;VirtualHost *:80&gt;
        ServerName example.com
        ServerAlias www.example.com
        ServerAdmin admin@example.com
        DocumentRoot /var/www/html
    . . .
step8> check the Permissions and Ownership Set Correctly
On Ubuntu and Debian, Apache and Nginx run as the user www-data which is a member of the www-data group.
To modify the ownership of a file, you can use chown:

       $ sudo chown user_owner:group_owner /path/to/file
This can also be done to a directory. You can change the ownership of a directory and all of the files under it by passing the -R flag:

       $ sudo chown -R user_owner:group_owner /path/to/file
       
If all else Fails, Check the Logs Again



         
