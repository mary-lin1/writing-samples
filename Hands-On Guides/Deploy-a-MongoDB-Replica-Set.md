# Deploy a MongoDB Replica Set
 
## Introduction
 
In this lab, students are asked to deploy a MongoDB replica set with three members. After completing this lab, students will have hands-on experience deploying a MongoDB replica set and querying primary and secondary members.
 
## Solution
 
### Configure and Start the mongod Service on Each Node

#### Connect to Node0 and Configure mongod
 
 1. Log in to the Node0 server using the credentials provided:

    ```
    ssh cloud_user@<PUBLIC_IP_ADDRESS>
    ```
    
 2. Edit the `mongod` configuration file:
 
    ```
    sudo vi /etc/mongod.conf
    ```
    
    **Note:** When copying and pasting code into Vim from the lab guide, first enter `:set paste` (and then `i` to enter Insert mode) to avoid adding unnecessary spaces and hashes. To save and quit the file, press **Escape** followed by `:wq`. To exit the file **without** saving, press **Escape** followed by `:q!`.
 
 3. When prompted, enter the `cloud_user` password.
 4. In the configuration file, scroll down to `# network interfaces` and add the private IP address to `bindIp`:
 
    ```
    bindIp: 127.0.0.1,10.0.0.101
    ```
    
 5. Scroll down to `#replication`, remove the `#`, and set the replica set name as `ReplicaSet0`:
 
    ```
    replication:
      replSetName: "ReplicaSet0"
    ```
 
 6. Save and exit by pressing the **Escape** key and entering:
 
    ```
    :wq
    ```
 
 7. Start the `mongod` service:
 
    ```
    sudo service mongod start
    ```
 
 8. Check the status of the service:
 
    ```
    sudo service mongod status
    ```
#### Connect to Node1 and Configure mongod
 
 1. Log in to the Node1 server using the credentials provided:

    ```
    ssh cloud_user@<PUBLIC_IP_ADDRESS>
    ```
    
 2. Edit the `mongod` configuration file:
 
    ```
    sudo vi /etc/mongod.conf
    ```
 
 3. When prompted, enter the cloud_user password.
 4. In the configuration file, scroll down to `# network interfaces` and add the private IP address to `bindIp`:
 
    ```
    bindIp: 127.0.0.1,10.0.0.102
    ```
    
 5. Scroll down to `#replication`, remove the `#`, and set the replica set name as `ReplicaSet0`:
 
    ```
    replication:
      replSetName: "ReplicaSet0"
    ```
 
 6. Save and exit by pressing the **Escape** key and entering:
 
    ```
    :wq
    ```
 
 7. Start the `mongod` service:
 
    ```
    sudo service mongod start
    ```
 
 8. Check the status of the service:
 
    ```
    sudo service mongod status
    ```

#### Connect to Node2 and Configure mongod
 
 1. Log in to the Node0 server using the credentials provided:

    ```
    ssh cloud_user@<PUBLIC_IP_ADDRESS>
    ```
    
 2. Edit the `mongod` configuration file:
 
    ```
    sudo vi /etc/mongod.conf
    ```
 
 3. When prompted, enter the `cloud_user` password.
 4. In the configuration file, scroll down to `# network interfaces` and add the private IP address to `bindIp`:
 
    ```
    bindIp: 127.0.0.1,10.0.0.103
    ```
    
 5. Scroll down to `#replication`, remove the `#`, and set the replica set name as `ReplicaSet0`:
 
    ```
    replication:
      replSetName: "ReplicaSet0"
    ```
 
 6. Save and exit by pressing the **Escape** key and entering:
 
    ```
    :wq
    ```
 
 7. Start the `mongod` service:
 
    ```
    sudo service mongod start
    ```
 
 8. Check the status of the service:
 
    ```
    sudo service mongod status
    ```

### Initiate the Replica Set

 1. On Node0, connect using the `mongosh` shell:
 
    ```
    mongosh
    ```
 
 2. Initiate the replica set:
 
    ```
    rs.initiate( {
      _id : "ReplicaSet0",
      members: [
        { _id: 0, host: "10.0.0.101:27017" },
        { _id: 1, host: "10.0.0.102:27017" },
        { _id: 2, host: "10.0.0.103:27017" }
      ]
    })
    ```
    
 3. If you received a response of `ok`, check the status to determine the primary node:
 
    ```
    rs.status()
    ```
 
 4. Scroll through the output to find the `members: [` section. The member at 10.0.0.101 will be the primary node.

### Test the Replication

 1. On Node0, create a database called `db`:
 
    ```
    use db
    ```
 
 2. Create a collection named `inventory`:
 
    ```
    db.inventory.insertMany( [
       { title: "A Very Good Book", author: "Arthur McWritesalot", published_date: "2010-01-01", publisher: "Good Publishing Inc.", available_copies: 2 }.
       { title: "A Book on Writing", author: "Dr. Arthur McWritesalot", published_date: "2010-01-14", publisher: "Good Publishing Inc.", available_copies: 4 },
       { title: "An Average Book", author: "Mathew Tryhard", published_date: "2015-01-14", publisher: "We Try Publishing Inc.", available_copies: 1 },
       { title: "Book on Grammar", author: "Susan Perfection", published_date: "2020-04-14", publisher: "Corrective Publishing", available_copies: 5 }
    ```
 
 3. Query the collection with the `find` command:
 
    ```
    db.inventory.find()
    ```
 
 4. Exit this connection and connect to the secondary server in the node on 10.0.0.102:
 
    ```
    mongosh mongodb://10.0.0.102:27017
    ```
 
 5. Switch to the `db` database:
 
    ```
    use db
    ```
 
 6. Query the collection:
 
    ```
    db.inventory.find()
    ```
    
 7. If you receive an error, change the read preference to `primaryPreferred`:
 
    ```
    db.getMongo().setReadPref("primaryPreferred")
    ```
 
 8. Query the collection again:
 
    ```
    db.inventory.find()
    ```
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!
