image:
	docker build -t slurm-docker-cluster:21.08 .

image22:
	docker build -t slurm-docker-cluster:22.05 .

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
	docker volume rm slurm-docker-cluster_etc_munge slurm-docker-cluster_etc_slurm slurm-docker-cluster_slurm_jobdir slurm-docker-cluster_var_lib_mysql slurm-docker-cluster_var_log_slurm
