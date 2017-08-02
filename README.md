# AWS and Devops-studynotes
Could not open a connection to your authentication agent on trying ssh-add

 This is because ssh agent is not running
 
 Start the agent as 
    eval `ssh-agent -s`
The result is agent pid.

sample: [ec2-user@ip-XXX-XX-X-XXX tmp]$ eval `ssh-agent -s`

Agent pid 22955


VPC private instance cannot access Internet

I had following ACLs:

Private subnet ACL 
Inbound

100	80 (HTTP)	TCP	0.0.0.0/0	ALLOW	Delete

110	443 (HTTPS)	TCP	0.0.0.0/0	ALLOW	Delete

120	22 (SSH)	TCP	10.0.0.0/24	ALLOW	Delete

140	1024 - 65535	TCP	10.0.0.0/24	ALLOW	Delete

ALL	ALL	0.0.0.0/0	DENY



Outbound

100	80 (HTTP)	TCP	0.0.0.0/0	ALLOW	Delete

110	443 (HTTPS)	TCP	0.0.0.0/0	ALLOW	Delete

120	49152 - 65535	TCP	10.0.0.0/24	ALLOW	Delete

140	1024 - 65535	TCP	10.0.0.0/24	ALLOW	Delete

ALL	ALL	0.0.0.0/0	DENY	



Based on above rules I see NAT is receiving packets from private instance, but private instance is not receiving packets back.


After adding Rule#150 "150	ALL	TCP	0.0.0.0/0	ALLOW" on inbound/outbound, my private instance can access internet.

AWS CLI and  S3  with bash script.

Here is a skeletal script file which allows you to manipulate files in s3 based on a given date.

s3cmd ls s3://$1 | while read -r line;

  do
  
    lastMdifiedDate=`echo $line|awk {'print $1" "$2'}`
    
    lastMdifiedDate=`date -d"$lastMdifiedDate" +%s`
    
    givenDate=`date -d"$2" +%s`
    
    if [[ $lastMdifiedDate -lt $givenDate ]]
    
      then
      
        fileSelected=`echo $line|awk {'print $4'}`
        
        echo $fileSelected
        
        s3cmd <action> "$fileSelected"
 
        
    fi
    
  done;
  



