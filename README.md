Resources
https://cloudacademy.com/course/building-cloud-native-application/create-docker-network-and-containers/

Steps for Docker network and Storage
1. Check docker networks using following command;
    docker network list
2. Create a new docker network for application stack using following command;
    docker network create voteapp-net
3. Inspect the network and check IP details under IPAM section(Short for IP Management) running following command;
    docker network inspect voteapp-net
4. Start up containers for application stack
    Container launch sequence
    mongdo
    api
    frontend
5. Pull official MongoDb image from DockerHub using following command
    docker pull mongo
6. Launch Mongo Container on port 27017; Run the docker; the -p flag is used to publish port and -d indicates that containter is detached; the port 27017 is standard for mongo;
    Run the following command;
    docker run --name mongo --network voteapp-net -d -p 27017:27017 mongo
7. Launch API Container on port 8080; note we will be setting env connection string variable; run the following command;
    docker run --name api-golang --network voteapp-net --env MONGO_CONN_STR=mongodb://mongo:27017/langdb -d -p 8080:8080 voteapp/api-golang:v1
8. Launch Frontend Container on port 80; run the following command;
    docker run --name frontend-react --network voteapp-net -d -p 80:80 voteapp/frontend-react:v1
9. Get the IP address of API
    docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' api-golang
10. Check if its up and running
    curl -s localhost:8080/ok


Creating MongoDb and Collection
1. Download mongo db shell client
    curl -O https://fastdl.mongodb.org/osx/mongodb-shell-macos-x86_64-4.2.0.tgz
2. Run the tar command(tar commands are used for compression in linux)
    tar -xvf mongodb-shell-macos-x86_64-4.2.0.tgz 
3. Navigate to directory
    cd mongodb-shell-macos-x86_64-4.2.0
4. exectute client
    ./bin/mongo
5. Create the database using command below;
    use langdb;
6. Insert 3 language records using commands below;
    db.languages.insert({"name" : "go", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 16, "compiled" : true, "homepage" : "https://golang.org", "download" : "https://golang.org/dl/", "votes" : 0}});
    db.languages.insert({"name" : "java", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 2, "compiled" : true, "homepage" : "https://www.java.com/en/", "download" : "https://www.java.com/en/download/", "votes" : 0}});
    db.languages.insert({"name" : "nodejs", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 30, "compiled" : false, "homepage" : "https://nodejs.org/en/", "download" : "https://nodejs.org/en/download/", "votes" : 0}});
7. Check collection using command below;
    show collections;
8. Confirm documents exist by quering for all documents using command below;
     db.languages.find().pretty();
9. Exit mongo shell client


Full-end to End testing
1. Now that you have data in db, try curl languages collection
    curl -s localhost:8080/languages
2. Try for individual language
    curl -s localhost:8080/languages/go
3. Open browser from terminal using command below;
    open -a /Applications/Google\ Chrome.app/ http://localhost:80
4.

