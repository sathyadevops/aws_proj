# aws_proj

   Attach EBS-Volume for Linux machine
                         --------------------------------------------------------
Step 1: Create a new EBS-Volumes 

Step 2: Attach volume for specific instance

Step 3: login to your ec2 instance and list the available disks using the 

following command.
               # lsblk
            
Step 4: Check the volume data (Optional)
               # sudo file -s /dev/xvdf

(NOTE: If the above command output shows "/dev/xvdf: data",
             it means your volume is empty)

Step 5: Format the volume to ext4 filesystem.
               # sudo mkfs -t ext4 /dev/xvdf

Step 6: Create a directory to mount  new ext4 volume
               # sudo mkdir /newvolume

Step 7: Mount the volume to "newvolume" directory
               # sudo mount   /dev/xvdf    /newvolume/

Step 8: cd into newvolume directory
               # df -h /newvolume

to unmount the volume, you have to use the following command.
               # umount /dev/xvdf

                                                  AWS-RDS 
                                                 ----------------

Step-1: create a Mysql DB in AWS cloud

Step-2: Find uname, pwd & End point 

           End point:  mydb.crinwonw4ars.us-east-2.rds.amazonaws.com
           UName :     admin
           Password:  abcd1234

Step-3: Install mysql-client on EC2 Instance
                # apt-get update
                # apt-get install mysql-client -y
                # apt-get install mysql-client-core-5.7 -y 

Step-4: Connect to MySQL Server 
Syntax:
    # mysql  -u <user name> -h <End point> -p

Ex:
   # mysql -u admin -h mydb.crinwonw4ars.us-east-2.rds.amazonaws.com 

-p



                                       AWS-ELB (Elastic Load Balancer)
                                      -------------------------------------------------- 
Classic ELB:
===========
 Step-1:  Create two EC2 Insances in Two different Availbility Zones
                    1-Ubuntu (us-east-2a)
                    1-Ubuntu (us-east-2b)

 Step-2:  Install web-Server (apcahe2)
                     # apt-get update; apt-get install apache2 -y
                     # service apache2 start
                     # netstat -lntp
 
 Step-3:  Deploy index.html file 
                    # cd /var/www/html/
	   # rm index.html
	   # vi index.html
	<body bgcolor=pink>
	        <h1> Hello from Web-Server-1 </h1>
	        <h1> Hello from Web-Server-1 </h1>
	</body>

 Step-4:  Select "Load Balaners" in AWS
                --> select "Create Load Balancer"  --> "Classic Load Balancer"

               copy A-Record and use in Browser 
        "http://myclassicloadbal-.us-east-2.elb.amazonaws.com"


Application ELB:
===============

 Step-1:  Create Four EC2 Insances in Two different Availbility Zones
                    2-Ubuntu (us-east-2a)
                    2-Ubuntu (us-east-2b)

 Step-2:  Install web-Server (apcahe2)
                     # apt-get update; apt-get install apache2 -y
                     # service apache2 start
                     # netstat -lntp
 
 Step-3:  Deploy index.html file 
                    # cd /var/www/html/
	   # rm index.html
                    # vi bank/index.html
<body bgcolor=pink>
        <h1> ============== XYZ BANK ============= </h1>
        <h1> ************* BANK SAVINGS ************** </h1>


 Step-4:  Select "Target Groups" in AWS

               Create Two Target Groups ---> BANK-TG , LOANS-TG

               select "Bank-TG" --> Actions ----> "Register Instances"
               select Two Bank Instances --> "Add to Registered" --> "Save"

               select "Loans-TG" --> Actions ----> "Register Instances"
               select Two Loan Instances --> "Add to Registered" --> "Save"

 Step-5:  Select "Load Balaners" in AWS
          --> select "Create Load Balancer"  --> "Application Load Balancer"

 Step-6:  Select "Listeneres" in ELB ---> select View/ Edit rules
               select Insert rule ---> 

               path: *bank*   ---> Forword to : Bank-TG    --> save
               path: *loans*   ---> Forword to : Loans-TG  --> save


               copy A-Record and use in Browser 
        "myappelb-231090918.us-east-2.elb.amazonaws.com/bank"
        "myappelb-231090918.us-east-2.elb.amazonaws.com/loans"
