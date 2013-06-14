Cloudbees-Mongodb
=================
Inspired by: http://developer.cloudbees.com/bin/view/DEV/Node+Builds

I am using the excellent MongoDB in a project and decided to try using Cloudbees for 
Continuous Integration.  Using Mocha, I had BDD going on my VM, but couldn't run it on
Cloudbees because it required mongo.  Creating MockMongo was an option but the API is very extensive
and would probably prove too difficult to maintain.  Plus, testing against the actual persistence layer
has its advantages.

This script will install Mongo on the Cloudbees /scratch directory so it is available
during your build process.  

To incorporate it into the build, you run use-mongo first, and then you can start and stop mongo thusly:

```
mongod --smallfiles --dbpath /scratch/jenkins/db --logpath /scratch/jenkins/mongo/log/mongod.log --fork
# invoke your build, I use grunt (for now)
grunt
mongod --shutdown --dbpath /scratch/jenkins/db
```

My current, complete build script is **build-cloudbees.sh**:
```
# Install Node.js
curl -s -o use-node https://repository-cloudbees.forge.cloudbees.com/distributions/ci-addons/node/use-node
NODE_VERSION=0.10.4 \
 source ./use-node

# Install MongoDb
curl -s -o use-mongo https://raw.github.com/gmoon/cloudbees-mongo/master/use-mongo
MONGO_VERSION=2.4.4 \
 source ./use-mongo

npm install
npm install grunt
npm install grunt-cli
export PATH=$PATH:node_modules/.bin/

mongod --smallfiles --dbpath /scratch/jenkins/db --logpath /scratch/jenkins/mongo/log/mongod.log --fork
grunt
mongod --shutdown --dbpath /scratch/jenkins/db
```

TODO
----
* Perhaps it is best to delete the files after shutdown

LICENSE
-------

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
