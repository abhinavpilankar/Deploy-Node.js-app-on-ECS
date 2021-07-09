# Deploy-Node.js-app-on-ECS

Demo Steps

1. Install Docker on EC2
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
exit==> login again
docker info
docker --version

2. Install Node.js on EC2
Refer: https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-up-node-on-ec2-instance.html

3. Create an app directory and use NPM to create a package.json file and add the Express framework.
$ mkdir hello-world
$ cd hello-world
$ npm init
$ npm install --save express

4. Create an index.js for our app code.
$ touch index.js

5. Open a code editor and add some code for a simple hello world app.
var express = require('express')
var app = express()

app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.listen(8081, function () {
  console.log('app listening on port 8081!')
})

6. Note that the Node code is using port 8081. Then, run the app in Node and open it in a browser.
node index.js
Node should tell you the app is running on port 8081. Test the app by opening <publicdns>:8081.
If you see “Hello World” all is good.
  
7. Create a Dockerfile and add following code
FROM node:7
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
CMD node index.js
EXPOSE 8082
Note: Docker will expose this application at port 8082 when the container is running.
  
8. Create a ECR repository ‘hello-repo’ and create and push image via view Push Commands
Note. For more detailed steps on ECR & ECS, please refer: https://medium.com/@learning.dipali/containerize-microservices-with-amazon-ecs-596fff62dacf
  
9. Create ECS Cluster (linux+Networking)
10. Create Task Definition and add Container information with port mapping as Host Port: 8082 and Container Port: 8081
11. Open ports 8081 and 8082 in security group.
12. Run Task, and validate your running application.


