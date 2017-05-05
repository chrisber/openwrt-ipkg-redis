include env_make
ROOT := $(shell /bin/pwd)
NS = noonien
VERSION ?= 14.07 

REPO = openwrt-buildroot
NAME = openwrt-redis
INSTANCE = default

.PHONY: help 


help:
	@grep -h -P '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'


build: ## Builds the container
	@docker build -t $(NS)/$(REPO):$(VERSION) .

clean_build: ## Builds the docker container with caching disabled
	@docker build --no-cache -t $(NS)/$(REPO):$(VERSION) .

push:
	@echo "push disabled Bovine SDK privately ownd by Vodafone"
	#docker push $(NS)/$(REPO):$(VERSION)

run: ## Run a shell inside the docker continer 
	@docker run --rm --name $(NAME)-$(INSTANCE)  $(PORTS) \
	-v $(Full_PATH_TO_REDIS):/home/openwrt/openwrt/package/redis \
	$(NS)/$(REPO):$(VERSION) sudo -iu openwrt bash

shell: ## Run a shell inside the docker continer 
	@docker run --rm --name $(NAME)-$(INSTANCE) -i -t $(PORTS) \
	-v $(Full_PATH_TO_OPENWRT):/openwrt \
	-v $(Full_PATH_TO_REDIS):/home/openwrt/openwrt/package/redis \
	$(NS)/$(REPO):$(VERSION) sudo -iu openwrt bash

stop: ## Stops a running container
	docker stop $(NAME)-$(INSTANCE)

rm: ## Deletes a  container
	docker rm $(NAME)-$(INSTANCE)

release: build
	$(info "release disabled")
	#make push -e VERSION=$(VERSION)

default: help
