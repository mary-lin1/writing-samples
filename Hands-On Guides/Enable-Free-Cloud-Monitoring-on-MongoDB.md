# Enable Free Cloud Monitoring on MongoDB
 
## Introduction
 
In this lab, students are tasked with enabling free cloud monitoring on a MongoDB instance. After completing this lab, students will have hands-on experience enabling and access free cloud monitoring in MongoDB.
 
## Solution
 
Log in to the server using the credentials provided:

```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
 
### Enable Free Cloud Monitoring
 
 1. Connect to the MongoDB instance using the `mongosh` shell:
 
    ```
    mongosh
    ```
 
 2. Check the status of free monitoring:
 
    ```
    db.getFreeMonitoringStatus()
    ```
 
 3. Enable free monitoring:
 
    ```
    db.enableFreeMonitoring()
    ```
 
 4. Copy the URL after `url:` and paste it to a text file or other location for later use in this lab.
 
### Generate Traffic

 1. Generate some traffic by running `insertOne` to start creating a collection named `inventory`:
 
    ```
    db.inventory.insertOne( { title: "A Very Good Book", author: "Arthur McWritesalot", published_date: "2010-01-01", publisher: "Good Publishing Inc.", available_copies: 2 } )
    ```
    
 1. Generate some traffic by running `insertMany` commands to complete the `inventory` collection:
 
    ```
    db.inventory.insertMany( [ 
       { title: "A Very Good Book", author: "Arthur McWritesalot", published_data: "2010-01-01", publisher: "Good Publishing Inc.", available_cipies: 2 },
       { title: "A Book on Writing", author: "Dr. Arthur McWritesalot", published_date: "2020-01-14", publisher: "Good Publishing Inc.", available_copies: 4 }.
       { title: "An Average Book", author: "Matthew Tryhard", published_date: "2015-01-14", publisher: "We Try Publishing Inc.", available_copies: 3 }
       { title: "Book on Grammar", author: "Susan Perfection", published_date: "2000-04-14", publisher: "Corrective Publishing", available_copies: 5 }
       ] )
    ```
    
 3. Check the number of documents inserted into the collection:
 
    ```
    db.inventory.countDocuments()
    ```

### Review Monitoring Data

 1. Open a new web browser window or tab, preferably in Incognito mode.
 2. Paste the URL that you obtained and copied when monitoring was enabled earlier.
 3. Examine the dashboard of different metrics for the instance.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!
