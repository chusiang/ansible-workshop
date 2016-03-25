primary_node = main.centos

main: status

up:
	vagrant up $(primary_node)

run:
	vagrant provision $(primary_node)

docker:
	docker build  -f test/Dockerfile.centos6  -t wordpress12-centos6  .
	docker run -d --name wordpress12-centos6   wordpress12-centos6
	sleep 60
	IP=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' wordpress12-centos6) ; echo http://$IP:80/ > url
	cat url  |  xargs -t -n 1 curl -v --retry 5 --retry-max-time 120 -o result-wordpress12-centos6
	grep '<title>ANSIBLE_TEST' result-wordpress12-centos6

ssh:
	vagrant ssh $(primary_node)
	
status:
	vagrant status

clean:
	vagrant destroy $(primary_node)
	docker rm -f `docker ps -a | grep Exited | awk '{ print $$1 }'`
	-rm result-wordpress url setup.retry

push:
	git push origin feature/4th-hw-centos

