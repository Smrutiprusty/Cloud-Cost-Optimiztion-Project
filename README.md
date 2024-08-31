# Aws-Cost-Optimiztion-Project


# Efficient AWS Cost Management through Stale Resource Detection


# Problem :
Sometimes, developers create EC2 instances with volumes attached to them by default. For backup purposes, these developers also create snapshots. However, when they no longer need the EC2 instance and decide to terminate it, they sometimes forget to delete the snapshots created for backup. As a result, they continue to incur costs for these unused snapshots, even though they are not actively using them.

# Solution :
We're using AWS to save money on storage costs. We made a Smart Lambda function that looks at our snapshots and our EC2 instances. If Lambda finds a snapshot that isn't connected to any active EC2 instances, it deletes it to save us money. This helps us keep our AWS costs down.

# Note :
There are many similar problems like this. For instance, we might attach an Elastic IP to our EC2 instance but forget to delete the Elastic IP after terminating the EC2 instance. In such a case, the Elastic IP continues to incur costs for us.

# Steps :
# Step 1:
1.Log into your AWS Console.

2.Navigate to the EC2 Console.

3.In the Instances section, select 'Instances,' and then click on 'Launch Instance'.


![image](https://github.com/user-attachments/assets/d8f60427-adb1-407b-b756-dda18122f80e)

4.Next, navigate to the 'Elastic Block Store' section and select 'Volumes'.


![image](https://github.com/user-attachments/assets/0c290f1b-db94-4e23-8e11-cfab2781ba47)


5.You will notice that a default volume has already been created for us.

6.Next, click on 'Snapshots,' and then click the 'Create Snapshot' button. It will prompt you with a page that looks like this.


![image](https://github.com/user-attachments/assets/675c3d77-df6e-4497-b174-0a60258314ee)


7.In Volume ID section choose your default Volume ID created when we create instance.

8.Next, click 'Next,' provide a name for your Snapshot, and then scroll down and click 'Create Snapshot'.


![image](https://github.com/user-attachments/assets/6ac0bde7-1f0f-47ec-8320-6022cb3b3f89)



# Step 2 :

1.After creating a Snapshot, navigate to the Lambda Console..

2.You will see some options in the user interface, such as 'Create Function'.

3.Click on 'Functions'.



![image](https://github.com/user-attachments/assets/ffb55692-746c-41de-b70d-8daffe45b739)

4.Select 'Author from Scratch,' then enter the Function name, and choose the latest Python version.

5.Scroll down and click 'Create Function'.

6.After creating the function, scroll down, and you will see something like the image below..

![image](https://github.com/user-attachments/assets/1893d656-b999-4fce-93d1-8199172be92c)

7.Click on the 'Code' section.

8.Next, clear the existing code and replace it with the 'identify_stale_snapshots.py' code.


![image](https://github.com/user-attachments/assets/d69bf392-1407-40d8-9328-34467aa785b7)

9.Click 'Deploy' to save your changes, and then click 'Test.' It will prompt a page that looks like the one given below.

![image](https://github.com/user-attachments/assets/475e6984-d7c5-4155-b1e9-56658d7e498a)


10.Please configure the settings as displayed above and then scroll down. Next, click on 'Create Event'.

11.Once you've created the event, proceed to the IAM Console(Identity and Access Management) and then navigate policies section to create a new policy.


![image](https://github.com/user-attachments/assets/26a55de7-b8e4-4ddf-a16e-67ba80ae4161)

12.Select the service as 'EC2'

![image](https://github.com/user-attachments/assets/44fac487-52aa-4a04-be1f-379a9ddd31f1)


13.In the 'Actions' section, grant permissions for the following actions: DescribeInstances, DescribeVolumes, DescribeSnapshots, DeleteSnapshots.

14.After Creating the Policies , Navigate to the Lambda Sections.

15.Next, go to the page of the Lambda function you've created. In the "Permissions" section, click on the role name.


![image](https://github.com/user-attachments/assets/758f734a-7a64-4a68-8879-168d6cc2ecf9)

16.Click on 'Add Permissions' and then select 'Attach Policy.'


![image](https://github.com/user-attachments/assets/cbde78bf-bb92-49b6-8a38-cc4bba9613b0)


17.Choose the correct policy you created.

![image](https://github.com/user-attachments/assets/ea50c862-9e85-4921-915b-880366595379)

18.Then scroll down and click 'Add Permissions'.

19.After that, you can go to the Lambda function page and run the code; it will display some outputs as shown below.

![image](https://github.com/user-attachments/assets/dbb9c67c-5554-45c3-afc4-4cca669a2f2f)


# Step 3 :
1.You can terminate the EC2 instance to test our Lambda function.

2.Navigate to the EC2 console and then terminate the EC2 instance.

3.Return to the Lambda console to test the code; go to the Lambda Function page.

4.Under the Code section, click 'Test code', it will display an output like this.

![image](https://github.com/user-attachments/assets/f6802899-888e-40e0-95bd-e9d01a8e8477)

5.As expected, our Lambda function deleted the snapshot because it was associated with a volume that couldn't be found.


# Additional notes:

We can use CloudWatch to automatically trigger the Lambda function every hour, day, minute, or second. However, this may result in higher costs because our Lambda execution time increases when triggered automatically. Nevertheless, manually triggering this function is a better choice because it allows us to trigger it when needed.

# CloudWatch or EventBridge Implementation :

# Steps :

1.Navigate to CloudWatch Console.

![image](https://github.com/user-attachments/assets/34139ac6-d1fd-48c2-b752-208fc8338b5a)

![image](https://github.com/user-attachments/assets/22335af2-c896-4a5c-9ab0-0316acef18cc)

![image](https://github.com/user-attachments/assets/6d030e87-7a94-4a84-98af-fc40f1f36244)

![image](https://github.com/user-attachments/assets/aef2f5a4-4e44-4ee9-aa89-288eb599ba73)


2.Next, on the following page, configure the schedule pattern as follows:

![image](https://github.com/user-attachments/assets/e2db4736-10ab-4259-bad2-ac094c2d64e4)

3.Scroll Down and then Click Next.

![image](https://github.com/user-attachments/assets/05e63e35-8002-4580-b6a9-ca16383e5ce6)

![image](https://github.com/user-attachments/assets/4201d1ef-6294-40cb-aedf-2effb34d7070)

4.Scroll Down and then Click Next.

5.On the next page, choose 'None' for the 'Action after Schedule' option.


![image](https://github.com/user-attachments/assets/5d2af129-b436-4dca-b01f-307de54ea4cf)

![image](https://github.com/user-attachments/assets/d1f68c02-9b2d-4f62-b6ab-e5ad3e07b1ac)

![image](https://github.com/user-attachments/assets/35e61c2a-0668-429a-a297-ed25f5e2f66a)

6.We have successfully created the scheduler, which will trigger the Lambda function every hour.

7.However, please note that this setup will incur some costs since the function is triggered continuously every hour. Alternatively, we can configure it to run on specific days and times as needed.

























