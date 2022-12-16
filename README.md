# slurm-in-docker

# Images

- 1. build cloudmesh-rockylinux
   - 1.1 install mpi in c1, c2, slurmctld
   - 1.2 install pympi c1, c2, slurmctld
   - 1.3 clone the cloudmesh-mpi dir here
   - 1.3 fetch the pdf file of tutorial and place in homedir 

- 2. use cloudmesh rockylinux in c1, c2, slurmctld

## ports/rodrigo

This port includes a docker compose file reusing an existing image
from dockerhub.

The description of this project is located at:

* <https://medium.com/analytics-vidhya/slurm-cluster-with-docker-9f242deee601>


## References

* <https://github.com/giovtorres/slurm-docker-cluster>
  @misc{giovtorres2022Dec,
	author = {giovtorres},
	title = {{slurm-docker-cluster}},
	howpublished = {GitHub},
	year = {2022},
	month = dec,
	note = {[Online; accessed 16. Dec. 2022]},
	url = {https://github.com/giovtorres/slurm-docker-cluster}
}

* @article{Ancavil2022Jan,
	author = {Ancavil, Rodrigo},
	title = {{Slurm Cluster with Docker - Analytics Vidhya - Medium}},
	howpublished = {Medium},
	year = {2022},
	month = jan,
	publisher = {Analytics Vidhya},
	url = {https://medium.com/analytics-vidhya/slurm-cluster-with-docker-9f242deee601}
}
* @misc{Scidas2022Dec,
	author = {Scidas},
	title = {{slurm-in-docker}},
	howpublished = {GitHub},
	year = {2022},
	month = dec,
	note = {[Online; accessed 16. Dec. 2022]},
	url = {https://github.com/SciDAS/slurm-in-docker}
}
* @misc{BibEntry2022Dec,
	title = {Planungsbasierter SLURM},
	howpublished = {GitLab},
	year = {2022},
	month = dec,
	note = {[Online; accessed 16. Dec. 2022]},
	url = {https://git.imp.fu-berlin.de/becker29/planungsbasierter-slurm/-/tree/dev/slurm-docker-cluster}
}