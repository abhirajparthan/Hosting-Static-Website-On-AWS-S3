# Hosting Static Website On AWS S3

-----
## Description
In this article, I will help you with how to host a simple static website on S3 and how to access the website using a specific name. We will configure an S3 bucket to provide users access to our website.

Amazon S3 is a service offered by Amazon Web Services that provides object storage through a web service interface. It provides unlimited storage for various use cases at a very low cost.

-----
## Prerequisite
AWS account with management console access and full access to S3 service.

------
## Table of Contents
1. Create an S3 bucket
2. Upload web files to S3 bucket
3. Setting Bucket Policy for public access
4. Enabling static website hosting for bucket
5. Point website url to bucket name using Rout 53.

Now, let’s get into it!

## Step-1 - Create an S3 bucket

login into your AWS management console and search S3 on the top navbar. Then select S3 from the drop-down.

From the S3 dashboard, click on Create bucket. Give the bucket a unique name, the name you choose must be globally unique, 

![Screenshot from 2022-03-14 11-01-27](https://user-images.githubusercontent.com/100773790/158110903-6367847b-4ad4-4f57-ab6f-c6a46401fea1.png)

Next, choose your preferred AWS Region from the drop-down. Here my bucket name is "static.abhiraj.ga" and my Region "ap-south-1"

![Screenshot from 2022-03-14 11-04-56](https://user-images.githubusercontent.com/100773790/158111226-53d2327a-25ff-4af6-ba4b-acc18f12935d.png)

Next, Uncheck the “Block all public access” button and acknowledge that you are aware that objects in this bucket will be publicly accessible over the internet by everyone.

![Screenshot from 2022-03-14 11-07-41](https://user-images.githubusercontent.com/100773790/158111549-1bcfa143-0e4f-4273-bed1-4b747f145c0e.png)

Then You can Add tag to the bucket for easy identification.

![Screenshot from 2022-03-14 11-10-51](https://user-images.githubusercontent.com/100773790/158111883-d3a256da-979e-47a5-9f7e-88dbc83ce2fe.png)

Then click "Create bucket" and you can see the bucket successfully created.

![Screenshot from 2022-03-14 11-13-06](https://user-images.githubusercontent.com/100773790/158112074-f03b2d07-f280-46de-bc07-d6abf14875a5.png)


## Step-2 - Upload web files to S3 bucket

Back to the S3 dashboard, click on the newly created bucket. On the objects, tab click “upload” to upload the static website files from the local computer.

![Screenshot from 2022-03-14 11-19-39](https://user-images.githubusercontent.com/100773790/158112717-86fe59b3-49fb-48f5-bda5-ca7437cbe387.png)

Click on “add file” to select the files you want to upload.

![Screenshot from 2022-03-14 11-21-15](https://user-images.githubusercontent.com/100773790/158112997-001e0052-ccbc-4ff5-9a9d-434082db6a7f.png)

After the necessary files and folders have been added, scroll down and click on Upload.

## Step 3 - Setting Bucket Policy for public access

Click on the bucket. On the Permissions tab, scroll down to Bucket Policy.

![Screenshot from 2022-03-14 11-28-20](https://user-images.githubusercontent.com/100773790/158113695-5bebf46b-e336-45fa-b9c9-9a05d90050bd.png)

Then edit bucket policy.

![Screenshot from 2022-03-14 11-29-38](https://user-images.githubusercontent.com/100773790/158113820-49dd9fc1-ed92-467d-a17d-87371121fec4.png)

Delete everything currently in the text field and paste the follow JSON text. Click “Save changes” when satisfied.

~~~
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::Bucket-Name/*"
        }
    ]
}
~~~

Make sure you replace Bucket-Name with the name of the bucket you created. This policy grants everyone over the internet to have access to the bucket and view our static website.

## Step-4 - Enabling static website hosting for bucket

Click on the bucket. On the Properties tab, scroll to the very bottom of the page to Static Website Hosting.

![Screenshot from 2022-03-14 11-35-43](https://user-images.githubusercontent.com/100773790/158114417-e2e8cb25-b460-4459-9d72-2d32a5a66283.png)

Click "edit"

![Screenshot from 2022-03-14 11-36-19](https://user-images.githubusercontent.com/100773790/158114543-77b0d49d-caab-4425-8135-a4982868a07e.png)

Then enable the following settings as below and scrool down.

![Screenshot from 2022-03-14 11-37-45](https://user-images.githubusercontent.com/100773790/158114629-6736c622-b630-43c1-89e4-2e6e3d9472da.png)

You can see the    Enter the file for your Index document and Error document. The Error document is optional. I used index.html for Index document. Then scrool down and click “Save changes”.

![Screenshot from 2022-03-14 11-40-59](https://user-images.githubusercontent.com/100773790/158114968-1fae378b-bfcd-456b-9eb2-e44bf436e8ea.png)

For getting the website URL. Go to bucket and Click on the Properties tab and scroll to the bottom of the page to Static Website Hosting. Click on the Bucket website endpoint link.

![Screenshot from 2022-03-14 11-43-40](https://user-images.githubusercontent.com/100773790/158115295-32bd89d5-34fa-453a-aff4-2343a30b155e.png)

You can see your website was online and the website link like this " http://static.abhiraj.ga.s3-website.ap-south-1.amazonaws.com". 

## Step-5 Point website url to bucket name using Rout 53

We have set up a static website with the S3 with the URL "http://static.abhiraj.ga.s3-website.ap-south-1.amazonaws.com". Here we discuss how to convert this url to bucket name ( specific name ) using Rout 53. 
~~~
eg: 
http://static.abhiraj.ga.s3-website.ap-south-1.amazonaws.com  >  static.abhiraj.ga
~~~

Amazon Route53 is a highly available and scalable DNS service. I have already configured my domain "abhiraj.ga" in the rout 53. I suggest you to navigate to rout 53 and add a A record for your domain static.abhiraj.ga and enable Alias.

![Screenshot from 2022-03-14 12-05-53](https://user-images.githubusercontent.com/100773790/158117844-8396d446-3050-467a-94f3-c12cdd14c6b2.png)

Then add the end point to "Alias to S3 website endpoint" and choose region "ap-south-1" ( because we are created the bucket in the ap-south-1 ). Then add ent point name ( click endpoint and you can see the bucket name with the region ) and create a record.

![Screenshot from 2022-03-14 12-11-00](https://user-images.githubusercontent.com/100773790/158118477-f69eb540-a32c-4bd2-ae62-48a33450c536.png)

Now you can acces your website through your bucket name like static.abhiraj.ga.


## Conclusion

This tutorial we discussed how to host a static website on AWS S3 and how to point the website URL to a specific name using Rout53. The goal is to get you started on using S3 to host a simple static website as it is cheap and easy to do.

### ⚙️ Connect with Me

<p align="center">
 <a href="https://www.instagram.com/_r.e.b.e.l.z_33/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/abhiraj-parthan-82038b191"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 









