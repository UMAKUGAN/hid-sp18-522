SWAGGER_CODEGEN=java -jar swagger-codegen-cli.jar
YML=Iris.yaml
HOME=server
CLIENT=client
CONFIG=$(HOME)/config
DATA=$(HOME)/data
CONTR=$(HOME)/swagger_server/controllers


docker-build:
	docker build -t saurabh/swagger .

docker-start:
	docker run -p 8080:8080 saurabh/swagger
	
docker-stop:
	docker stop $$(docker ps -a -q -f status=running)

service:
	#creates the swagger service from the yaml file and places the controllers in the appropriate directory
	$(SWAGGER_CODEGEN) generate -i $(YML) -l python-flask -o $(HOME) -D supportPython2=true
	cp *_controller.py $(CONTR)
	cp requirements.txt $(HOME)
	# Install dependencies and server side code
	cd $(HOME); pip install --upgrade pip; pip install -r requirements.txt; python setup.py install; cd ..;
	
	
start:
	#starts the services
	cd $(HOME); python -m swagger_server; cd ..;

stop:
	#stops the service
	ps -ef | grep swagger | grep -v "grep"| awk '{print $$2}' | xargs kill -9


test-download:
	echo "Testing data API"
	curl -H "Content-Type:application/json" -X GET http://localhost:8080/Iris/Download

test-categories:
	echo "Testing data-fetch API"
	curl  -H "Content-Type:application/json" -X GET http://localhost:8080/Iris/categories

test-categoryname:
	echo "Testing data-fetch API"
	curl  -H "Content-Type:application/json" -X GET http://localhost:8080/Iris/categories/Iris-setosa
	
clean: 
	# removes the code generated
	if [ -d $(HOME) ]; then rm -r $(HOME); fi
	

