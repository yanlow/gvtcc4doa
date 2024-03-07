CONTAINER_NAME = $(shell basename $(shell pwd))

build:
	docker build -t $(CONTAINER_NAME) .

up:
	docker run --rm -d \
	-h docker -e "TERM=xterm-256color" \
	--name $(CONTAINER_NAME) \
	$(CONTAINER_NAME)

down:
	docker rm -f $(CONTAINER_NAME)

conn:
	docker exec -it $(CONTAINER_NAME) /bin/bash
