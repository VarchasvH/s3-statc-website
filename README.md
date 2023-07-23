# Creating a Static Website for a Café using AWS S3
In this Project, We use Amazon Simple Storage Service (Amazon S3) to build a static website and implement architectural best practices to protect and manage our data. All the project files are present int the 'static-website.zip' folder.

---

## Project Scenario

Anmol and Ribhu are a husband-and-wife team who owns and operate a small café business that sells desserts and coffee. Their daughter, Laskshita, and their other employee, Rohan—who is a secondary school student—also work at the café. The café has a single location in a large city.

The café currently doesn’t have a marketing strategy. They mostly gain new customers when someone walks by, notices the café, and decides to try it. The café has a reputation for high-quality desserts and coffees, but their reputation is limited to people who have visited, or who have heard about them from their customers.

Laskshita suggests to Anmol and Ribhu that they should expand community awareness of what the café has to offer. The café doesn’t have an online presence yet, and it doesn’t currently use any cloud computing services. However, that situation is about to change.

---

## What we will learn by doing this Project
* Hosting a static website by using Amazon S3.
* Implementing one way to protect your data with Amazon S3.
* Implementing a data lifecycle strategy in Amazon S3.
* Implementing a disaster recovery (DR) strategy in Amazon S3.

---

# End Goal
At the end our architecture should look like the following example:
![Screenshot 2023-07-23 121644](https://github.com/VarchasvH/s3-statc-website/assets/100064742/6229f846-391c-43c6-b630-38d1112cce4b)



# STEPS TO FOLLOW
## STEP 1: A Business request for the café: Launching a static website

Lakshita mentions to Rohan that she would like the café to have a website that will visually showcase the café's offerings. It would also provide customers with business details, such as the location of the store, business hours, and telephone number.

Rohan is happy that he was asked to create the first website for the café.

For this first challenge, you will take on the role of Rohan and use Amazon S3 to create a basic website for the café.

### Task 1: Extract the website files on your local machine

1. Visit this GitHub Repo and download the 'static-website.zip' file.
    
2. Extract it onto your local machine
    
3. Make sure that you have an *index.html* file and two folders that contain Cascading Style Sheets (CSS) and image files.
    

### Task 2: **Creating an S3 bucket to host our static website**

1. Create a Bucket in any region.
    
2. Disable all public access.
    
3. Enable static website hosting on our bucket.
    ![Screenshot 2023-07-22 182713](https://github.com/VarchasvH/s3-statc-website/assets/100064742/5a5ff298-2b2f-4889-9cc4-76527eda9ff9)
   

### Task 3: **Uploading content to our S3 bucket**

1. Upload the 'index.html' file, the CSS and the 'images" folders to your S3 bucket.
    
2. Open the endpoint link for your static website on an incognito window.
    
    <div data-node-type="callout">
    <div data-node-type="callout-emoji">💡</div>
    <div data-node-type="callout-text">To access the endpoint link, Click Bucket -&gt; Properties -&gt; Static Website Hosting and enable it using the 'index.html' file.</div>
    </div>
    
3. Your access would be denied for now.

### Task 4: Creating a bucket policy to grant public read access

1. Create a bucket policy that grants read-only permission to public anonymous users by using the Bucket Policy editor.
    
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": {
                    "AWS": "*"
                },
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::website-static-varchasv/*"
            }
        ]
    }
    ```
    
2. Confirm that the website for the café is now publicly accessible and it should look something like this.
    ![Screenshot 2023-07-23 121644](https://github.com/VarchasvH/s3-statc-website/assets/100064742/9074aa50-057f-4b3f-81f1-6c3a16895200)



---

## STEP 2: New business requirement: Protecting website data

You show Lakshita the new website, and she's very impressed. Good job!

You and Lakshita discuss that you will likely need to make many updates to the website as the number of café offerings expands.

Pakhru, an AWS Solutions Architect and café regular, advises you to implement a strategy to prevent the accidental overwrite and deletion of website objects.

You already need to make some changes to the website, so you decide that this would be a good time to explore object versioning.

### Task 5: **Enabling versioning on the S3 bucket**

1. Click on Bucket Properties and enable Bucket Versioning.
    
2. Make some bgcolor changes to the index.html file on your favorite editor.
    
3. Upload the updated file on the bucket and we can now see different versions of the 'index.html' file.
    

---

## STEP 3: New business requirement: Optimizing costs of S3 object storage

Now that you enabled versioning, you realize that the size of the S3 bucket will continue to grow as you upload new objects and versions. To save costs, you decide to implement a strategy to retire some of those older versions.

### Task 6: Setting lifecycle policies

1. Configure two rules in the website bucket's lifecycle configuration.
    
    * In one rule, move previous versions of all source bucket objects to S3 Standard-IA after 30 days.
        
    * In the other rule, delete previous versions of the objects after 365 days.
        ![Screenshot 2023-07-22 193736](https://github.com/VarchasvH/s3-statc-website/assets/100064742/ccd22f57-c484-4ce5-bc11-6d8aac1e3620)


        
2. By doing this, we implemented the architecture best practice of defining data lifecycle management.
    

---

## STEP 4: New business requirement: Enhancing durability and planning for DR

The next time Pakhru comes to the café, you tell her about the updates to the website. You describe the measures that you took to protect the website's static files from being accidentally overwritten or deleted. Pakhru tells you that cross-Region replication is another feature of Amazon S3 that you can also use to back up and archive critical data.

### Task 7: Enabling cross-Region replication
![Screenshot 2023-07-22 194520](https://github.com/VarchasvH/s3-statc-website/assets/100064742/ebd48b38-812a-4d42-a403-0c66deb8e2a7)



1. In a different Region than our source bucket, create a second bucket and enable versioning on it. The second bucket is our *destination bucket*.
    
2. On our Source Bucket, we will enable cross-region replication and create a new Replication rule that:
    
    * Replicates the entire source bucket.
        
    * Uses the **CafeRole** for the AWS Identity and Access Management (IAM) role.
        
3. Make a minor change to the *index.html* file and upload the new version to your source bucket.
    
4. Verify that the source bucket now has three versions of the *index.html* file.
    
5. Confirm that the new object was replicated to your destination bucket. You might need to reload the browser tab.
    
6. Go to your source bucket and delete the latest version.
    

In this task, we implemented the architecture best practice of *automating disaster recovery*.

---
# Conclusion

Congratulations, you helped a small business in making its online identity using some basic AWS tools. Be Proud of Yourself!


If you enjoyed this project, then be sure to like and share it with your friends and colleagues and if you have any doubts you can comment or you can reach out to me on Twitter or LinkedIn.




    











