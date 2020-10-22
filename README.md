# MongoDB Dockerfile

## Implementation decisions
At this moment the solution requires to build and start the docker, and then to run the command `mongorestore` in order to restore the database.

**NOTE:** Ideally, the dockerfile would copy the file to be imported and would provide a smothlier experience, but for the sake of simpliciy and reusability with other applications, I have decided to let it like this.

## Build and Run

```bash
docker build -t mongo_docker .
docker run -d -p 27017:27017 --name mongodb mongo_docker
```

## Usage

### Run `mongod`
    docker run mongo_docker -p 27017:27017

    docker run -d -p 27017:27017 --name mongodb mongo_docker

### Run `mongod` w/ persistent/shared directory

    docker run -d -p 27017:27017 -v <db-dir>:/data/db --name mongodb dockerfile/mongodb

### Run `mongod` w/ HTTP support

    docker run -d -p 27017:27017 -p 28017:28017 --name mongodb dockerfile/mongodb mongod --rest --httpinterface

### Run `mongod` w/ Smaller default file size

    docker run -d -p 27017:27017 --name mongodb dockerfile/mongodb mongod --smallfiles

### Run `mongo`

    docker run -it --rm --link mongodb:mongodb dockerfile/mongodb bash -c 'mongo --host mongodb'

This will allow you to connect to your mongo container with the standard `mongo` commands.

## Install mongo cli commands locally

To run the following commands, you need to install mongo in your computer. You can do so with the following commands (in MacOS):
```bash
brew tap mongodb/brew
brew install mongodb-community@4.4
```

**NOTE:** it may require to install [homebrew](https://brew.sh/index_es) if you don't have it already.

## Import and export data

Import database:
```bash
mongorestore yoga_solo_bckp
```

Export database:
```bash
mongodump -d yoga_solo -o yoga_solo_bckp
```

## Mongo commands

Import file as collection in database:
```bash
mongoimport -d yoga_solo -c postures --file postures.json --jsonArray
```

List the databases:
```bash
show dbs
```

Create database does not require to create a database itself, but just start using it:
```bash
use yoga_solo
```

List all collections in database:
```bash
show collections
```

Create an item in a new collection:
```bash
db.test.insertOne( { "name": "Christian", "position": "Software Developer Engineer" } )
```

List items in collection in a readable way:
```bash
db.test.find().pretty()
```

Remove an entire collection:
```bash
db.test.drop()
```

Remove an item in a collection:
```bash
db.test.remove()
```
