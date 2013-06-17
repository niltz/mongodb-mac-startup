# MongoDB Startup Scripts for OS X

This is a very basic startup script for running MongoDB on Mac OS X with multiple shards.

It is intended to run the defaults and without any authentication, but remain easy to
modify to suit your needs. Feel free to fork to add features.

## Dependencies

Make sure to install MongoDB from here:

  http://www.mongodb.org/display/DOCS/Downloads
  
The script assumes you have installed mongodb to

	/usr/local/mongodb
	
You will also need to create the following folders

	/usr/local/mongodb/log
	/usr/local/mongodb/db/a
	/usr/local/mongodb/db/b
	/usr/local/mongodb/db/config
	/usr/local/mongodb/db/routing

## Running

Using sudo or as root, copy the MongoDB directory and all its contents 
to /Library/StartupItems/ then start it like this:

	sudo /Library/StartupItems/MongoDB/MongoDB start
	
## Setup

Once mongo is running, you will need to connect to the mongos instance and configure the shards:

    $ mongo
    mongos> sh.addShard("localhost:10000");
    mongos> sh.addShard("localhost:10001");
    
You should only need to do this once. Now everything should be configured and working. However, by default, new
databases/collections are not sharded and will only exist on the first shard. To enable sharding for a
database/collection you need to do the following:

    mongos> sh.enableSharding("<database>");

This enables sharding for the database, but no sharding is performed

    mongos> sh.shardCollection("<database>.<collection>", shard-key-pattern);

A simple shard-key-pattern is { "_id": 1 }. This will now enable sharding on the collection using the shard key.
    
For more information see:

   http://docs.mongodb.org/manual/tutorial/deploy-shard-cluster/

## TODO

* Work with mongodb configuration files
* Handle replication (maybe)

## Copyright and License

MongoDB Startup Script for OS X
Copyright (C) 2012 Jeff Sawatzky

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
