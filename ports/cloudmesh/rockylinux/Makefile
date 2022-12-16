CM=$(shell find  ~ -type d -name 'cm' | head -n 1)
USERNAME=$(shell basename $$HOME)
VERSION=latest

#PROGRESS=--progress plain

PROGRESS=--progress auto

UID=$(shell id -u)
GID=$(shell id -g)
PWD=$(shell pwd)

export UID
export GID
image:
	docker build -t cloudmesh-rockylinux:8 .

shell:
	@echo ${UID}
	@echo ${GID}
	@echo ${HOME}
	docker run \
	  --name cloudmesh-rockylinux:8 \
	  -it \
	  --user ${UID}:${GID} \
	  /bin/bash

a:
	  -p 8000:8000 \
	  --rm  \
	  --volume "${CM}:${HOME}/cm" \
	  --volume "${HOME}/.cloudmesh:${HOME}/.cloudmesh" \
	  --volume "${HOME}/.local:${HOME}/.local" \
	  -it \
	  --user ${UID}:${GID} \
	  --workdir="${HOME}/cm" \
	  --volume "${HOME}/.ssh:${HOME}/.ssh:ro" \
	  --volume="/etc/group:/etc/group:ro" \
	  --volume="/etc/passwd:/etc/passwd:ro" \
	  --volume="/etc/shadow:/etc/shadow:ro" \
	  cloudmesh/python:${VERSION} \
	  /bin/bash

login:
	docker exec -it cloudmesh-rockylinux bash

node:
	docker exec -it c1 bash
