all: image run register start

image:
	docker build -t cloudmesh-slurm:21.08 .

run:
	docker-compose up -d

register:
	./register_cluster.sh

shell:
	docker exec -it slurmctld bash

node:
	docker exec -it c1 bash

stop:
	docker-compose stop

start:
	docker-compose start

kill: stop

test:


clean:
	docker-compose stop
	docker-compose rm -f
	docker volume rm cloudmesh-slurm_etc_munge cloudmesh-slurm_etc_slurm cloudmesh-slurm_slurm_jobdir cloudmesh-slurm_var_lib_mysql cloudmesh-slurm_var_log_slurm
