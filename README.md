# Devops-studynotes
Could not open a connection to your authentication agent on trying ssh-add
 This is because ssh agent is not running
 Start the agent as 
    eval `ssh-agent -s`
The result is agent pid: 
sample: [ec2-user@ip-XXX-XX-X-XXX tmp]$ eval `ssh-agent -s`
Agent pid 22955
