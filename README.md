use-mongo
=========
Inspired by: http://developer.cloudbees.com/bin/view/DEV/Node+Builds

Synopsis
--------
A simple build script to install MongoDB on your Jenkins slave.

Description
-----------
You have an app that depends on MongoDB.  So do your automated tests.  You want to run Continuous Integration
and want to build on a clean system, perhaps using Cloudbees.  

Here is a build script that uses this project to install MongoDB, and a similar project to intall Node.js:
```
#!/bin/sh

# Install Node.js
curl -s -o use-node https://repository-cloudbees.forge.cloudbees.com/distributions/ci-addons/node/use-node
NODE_VERSION=0.10.4 \
 source ./use-node

# Install MongoDb
curl -s -o use-mongo https://raw.github.com/gmoon/use-mongo/master/use-mongo
MONGO_VERSION=2.4.4 \
 source ./use-mongo

npm install
npm install grunt
npm install grunt-cli
export PATH=$PATH:node_modules/.bin/

mkdir -p ./mongo/log
mkdir -p ./mongo/db

mongod --smallfiles --dbpath ./mongo/db --logpath ./mongo/log/mongod.log --fork
grunt
mongod --shutdown --dbpath ./mongo/db
```

TODO
----
* Perhaps it is best to delete the files after shutdown

LICENSE
-------
```
Copyright 2013 George Moon

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```
