# Video-to-Audio-Converter---Microservice
Distributed microservice  - Video to Audio file conversion using  using Python, Kubernetes, RabbitMQ, MongoDB, mySQL


Instructions:

- Starting Kubernetes: minikube start
- For latest versions before building docker file in each services:
- Run following command:
   pip3 freeze > requirements.txt

- Authentication Service:

	cd to auth:

	Run following commands on terminal:
		source ./venv/bin/activate

	Database commands:
		Mysql -uroot > init.sql

	To view database:
		use auth;
		show tables;
		select * from user;

	# docker commands
		docker build .
		docker tag `SHA key from out put of docker build command` `dockerhub account name`/auth:latest
		docker push `dockerhub account name`/auth:latest

	cd to manifests:

	Run following commands on terminal:

		kubectl apply -f ./

 
- Gateway Service:

	cd to gateway:

	Run following commands on terminal:
		source ./venv/bin/activate
	# docker commands
		docker build .
		docker tag `SHA key from out put of docker build command` `dockerhub account name`/gateway:latest
		docker push `dockerhub account name`/gateway:latest

	cd to manifests:

	Run following commands on terminal:

		kubectl apply -f ./

- RabbitMQ Service:

	cd to rabbit:

	Run following commands on terminal:

	# docker commands
		docker build .
		docker tag `SHA key from out put of docker build command` `dockerhub account name`/rabbit:latest
		docker push `dockerhub account name`/rabbit:latest

	cd to manifests:

	Run following commands on terminal:

		kubectl apply -f ./


	minikube tunnel
	Enter laptop password
	# To view rabbit mq service

	Go to rabbitmq-manager.com
		Username: guest
		Password: guest


- Converter Service:

	cd to converter:

	Run following commands on terminal:
		source ./venv/bin/activate
	# docker commands
		docker build .
		docker tag `SHA key from out put of docker build command` `dockerhub account name`/converter:latest
		docker push `dockerhub account name`/auth:latest

	cd to manifests:

	Run following commands on terminal:

		kubectl apply -f ./



- Video to Audio Conversion by sending curl request:

Run following commands on Terminal:
	     - curl -X POST http://mp3converter.com/login -u `emailid in mysql db`: `password in mysql db`
	     - ex. curl -X POST http://mp3converter.com/login -u mrunmayeerane98@gmail.com:Admin123
	     - you will receive a key from above command 
	     - go to directory containing video
	     - curl -X POST -F 'file=@./videofilename' -H 'Authorization: Bearer (key)' http://mp3converter.com/upload
	     - Ex. curl -X POST -F 'file=@./test.mp4' -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Im1ydW5tYXllZXJhbmU5OEBnbWFpbC5jb20iLCJleHAiOjE2NzkxMDY2MzMsImlhdCI6MTY3OTAyMDIzMywiYWRtaW4iOnRydWV

9.mp-md8gpwf5lWptsXMSRQQjfmMX7lsFkZTAKwbBxJbo' http://mp3converter.com/upload
		- For viewing database:
		- Terminal Commands:
		    - mongosh
		    - show databases;
		    - usemp3s;

5. Go to rabbitmq-manager.com, use above credentials(guest)
	- go to Queues
	- Click on Get Message:
	- in payload mp3_fid is given as in screenshot:

<img width="1047" alt="ss" src="https://github.com/mrunmayee17/Video-to-Audio-Converter---Microservice/blob/e90469de3e1d79c971df7c009931f27a48c36bad/ss.png">

6. Use mp3_id to download audio file using curl request
	- Mongofiles --db=mp3s get_id --local=audiofilename.mp3 '{ $oid: "(mp3id)}'
	- Ex. mongofiles --db=mp3s get_id --local=test.mp3 '{"$oid": "6412e996ee8dfde2d885d62e"}' 


Ref: https://www.youtube.com/watch?v=hmkF77F9TLw



