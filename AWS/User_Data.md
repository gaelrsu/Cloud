# User Data
When an Amazon Elastic Compute Cloud (Amazon EC2) instance launches, it runs a user data script. Commands in the user data script are used to automate common configuration tasks.

## Check the User Data
### GIU
Select the instance -> Actions -> c hoose Instance settings -> Edit user data.
### CLI
Select the instance -> Connect -> Connect using EC2 Instance Connect -> Connect 
```
cat /var/lib/cloud/instance/user-data
```

## check logs 
```
sudo cat /var/log/cloud-init-output.log
 ```

## Edit the User Data
(stop the instance before) 
Select the instance -> Actions -> Instance settings -> Edit user data -> Save
OR 
Select the instance -> Connect -> Connect using EC2 Instance Connect -> Connect 
```
nano /var/lib/cloud/instance/user-data
```

## rerun the user data 
Select the instance -> Instance State -> Stop instance -> Stop  and then start it
OR 
Select the instance -> Connect -> Connect using EC2 Instance Connect -> Connect 
```
sudo /var/lib/cloud/instance/scripts/part-001
```
