# Add and Remove Shards on MongoDB Cluster

## Introduction

In this lab, students are asked to add and remove shards from a MongoDB sharded cluster.  After completing this lab, students will have hands-on experience configuring, adding, and removing shards from a MongoDB sharded cluster.

## Solution

Log in to the server using the credentials provided:

```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```
 
### Add a Shard
 
 1. Connect to the cluster using the `mongosh` client:
 
    ```
    mongosh mongodb://10.0.0.101:27017
    ```
 
 2. List the shards:
 
    ```
    db.adminCommand( { listShards: 1 } )
    ```
 
 3. Exit the `mongos` shell:
 
    ```
    quit
    ```
    
 4. Edit the `mongod` configuration file:
 
    ```
    sudo vi /etc/mongod.conf
    ```
 
 5. Under `# network interfaces`, add the internal IP address to the `bindIp` line:
 
    ```
    bindIp: 127.0.0.1,10.0.0.101
    ```
 
 6. Change the port to 27018:
 
    ```
    port: 27018
    ```
 
 7. Under `# sharding`, remove the `#` and set the `clusterRole`:
 
    ```
    sharding:
      clusterRole: shardsvr
    ```
 
 8. Under `# replication`, remove the `#` and set the replication name:
 
    ```
    replication:
      replSetName: shard2
    ```
 
 9. Save and quit by pressing the **Escape** button and entering:
 
    ```
    :wq
    ```

10. Start the `mongod` service:

    ```
    sudo service mongod start
    ```

11. Check the status of the service:

    ```
    sudo service mongod status
    ```

12. Connect to the newly configured instance using `mongosh`:

    ```
    mongosh mongodb://10.0.0.1:27018
    ```

13. Initiate the replica set:

    ```
    rs.initiate( {
      _id : "shard2",
      members: [
         { _id: 0, host: "10.0.0.101:27018" }
      ]
    })
    ```

14. Exit the `mongosh` shell:

    ```
    quit
    ```

15. Connect to the `mongos` instance running on port 27017:

    ```
    mongosh mongodb://10.0.0.101:27017
    ```

16. List the shards on the instance:

    ```
    db.adminCommand( { listShards: 1 } )
    ```

17. Add the shard running on the host ending in .103 with shard2 as the replica set name and 27018 as the port:

    ```
    sh.addShard( "shard2/10.0.0.101:27018")
    ```

18. List the shards on the instance:

    ```
    db.adminCommand( { listShards: 1 } )
    ```

### Remove a Shard

 1. Remove a shard, specifying `shard1` as the shard name:
 
    ```
    db.adminCommand( { removeShard: "shard1" } )
    ```
 
 2. Rerun the `removeShard` command to check the status of the chunks being migrated:
 
    ```
    db.adminCommand( { removeShard: "shard1" } )
    ```
 
 3. Continue running the `removeShards` command until zero chunks remain:
 
    ```
    db.adminCommand( { removeShard: "shard1" } )
    ```
 
 4. When the process has completed, list the shards on the cluster:
 
    ```
    db.adminCommand( { listShards: 1 } )
    ```
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!