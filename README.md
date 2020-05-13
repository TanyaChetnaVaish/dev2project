# PROJECT 2
## DevOps Assembly Lines (2020)

```
AIM:
1.Create container image that’s has Jenkins installed  using dockerfile.
2.When we launch this image, it should automatically starts Jenkins service in the container.
3.Create a job chain of job1, job2, job3 and  job4 using build pipeline plugin in Jenkins 
4.Job1 : Pull  the Github repo automatically when some developers push repo to Github.
5.Job2 : By looking at the code or program file, Jenkins should automatically start the respective language interpreter install image container to deploy code ( eg. If code is of  PHP, then Jenkins should start the container that has PHP already installed ).
6.Job3 : Test your app if it  is working or not.
7.Job4 : if app is not working , then send email to developer with error messages.
8.Create One extra job job5 for monitor : If container where app is running. fails due to any reson then this job should automatically start the container again.
```
***Assumptions: Git is installed on Windows. The user must have GitHub profile. Docker and Jenkins must be installed ,here it is installed on RHEL8 running on VirtualBox. Webserver and docker server is configured. There is active internet connection.Make sure you have Docker installed with required images -centos:latest,vimal13/apache-webserver-php and httpd.***
## STEPS
1.Create  a Dockerfile in a folder/directory (here its in /ws2) and write the following commands:
![](git/oo1.PNG)
![](git/oo2.PNG)
About the commands used in the Dockerfile:
```
FROM:to download the image to be used for container
RUN:commands to be executed while building the modified container
COPY:command will be used to copy files from host to image while building it.
CMD:the cmd used here will keep the Jenkins live till the container is on and will start on container boot.
```
The one.py file is made in /ws2 itself and the path is specified in the Dockerfile for the same.The one.pyfile is:
![](git/003.PNG)
In this file mention the sender email(your emailid) and password ,also the reciever emailid.Write the message you want to send when the build(job3) fails.
NOTE:EXPOSE 8080 command is optional you can do patting in the run command.python36 should be installed to run the one.py file.

2.Now build the image using following command:
![](git/o1.PNG)

Run the image now ,here I have done patting and specified a folder:
![](git/o2.PNG)

Now after running jenkins will start in this container image,and you will get the Administrator password displayed as well.(make sure you note it or copy this password!)
![](git/005.PNG)

Also you can get the port number by running the following command on the separate terminal window.(this command is useful when you havent dont patting that is specified port number yourself,used EXPOSE 8080)
![](git/o3.PNG)
3.Now open Jenkins GUI in your firefox on Redhat or in Chrome on Windows using your Redhat IP address(enp0s3) and port number(here 9999):
![](git/o4.PNG)
***Here in Administrator Password write the password you got after running the container***
Now create your account:
![](git/05.PNG)
Install the recommended Plugins,or choose the ones to download manually.

**CREATING JOBS AND BUILD PIPELINE**

***Create the local git repository (here project2dev) and make files in it of .php and .html extension and push it to GitHub using git remote.Also push the files onto GitHub,keeeping git status uptodate.***
![](git/first.PNG)

4.Create job1,give GitHub Repository URL,add trigger,here I added Remote trigger.
![](git/o6.PNG)
![](git/o7.PNG)
![](git/o8.PNG)
```
Here i have given the path of the folder in which files will be kept ,which are copied from GitHub
```
Save this.

5.Create job2,give GitHub repository URL,add build after other projects trigger
![](git/o9.PNG)
![](git/o10.PNG)
![](git/o11.PNG)
```
Here ,i have used sudo.Also checked if the os is already running.If running then deleted it and then started it again with the GitHub acquired files.Using if-else-then executed the task of choosing the container to run ,based on the file extension.I used vimal13/apache-webserver-php image since it already had webserver and php configured just as I want to perform the task!
```
Save this!

6.Create job3.This job will check the status of the page.If the page doesnt execute then this build will fail thus triggering job4 which will send email.
![](git/o12.PNG)
![](git/o13.PNG)

Also add a post-build action:
![](git/o15b.PNG)

This will trigger job4 if job3 fails.

After running this job you can notice your container running in Redhat Docker
![](git/o14.PNG)

7.Create job4 that will send email when out build fails.Here our one.py file comes in use to send email.
![](git/o15.PNG)

8.Finally create job5,which will run after our job4 fails to start conatiner agian.Use POLL SCM here.
![](git/o16.PNG)

Now Build Pipeline.For this download Build Pipeline Plugin.and then add job1 as initial job to run and thus save it!
Here it is your pipeline.On building it you will see your job successfully running till job3.Since job4 only runs when job3 fails so our pipeline stops there.

![](git/stable.PNG)
![](git/Webpage.PNG)

Again if you make any changes in job3 thjat is if job3 fails then our job4 build thus sending us email.
![](git/last.PNG)

Here is the email that i recieved.
![](git/email.PNG)

Thankyou!


