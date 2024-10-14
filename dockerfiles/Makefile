
# Help system from https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.DEFAULT_GOAL := help

help:
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

monero_sha := 51ba03928d189c1c11b5379cab17dd9ae8d2230056dc05c872d0f8dba4a87f1d
monero_version := v0.18.3.4

TRAILING_SLASH := "/"
define get-docker-registry
	ifeq ($(patsubst %$(TRAILING_SLASH),,$(2)),)
	else
		processed=$(2)/
		$(1) = $$(processed)
	endif
endef

$(eval $(call get-docker-registry,docker-registry,$(DOCKER_REGISTRY)))

check:
	$(eval package=monero)
	@echo ${docker-registry}
	@echo sudo docker buildx build --build-arg MONERO_VERSION=$(monero_version) --build-arg MONERO_HASH=$(monero_sha) --platform linux/amd64 -t $(docker-registry)$(package):$(monero_version) -t $(docker-registry)$(package):latest --push -f ./monero .

build-monero:
	$(eval package=monero)
	sudo docker buildx build --build-arg MONERO_VERSION=$(monero_version) --build-arg MONERO_HASH=$(monero_sha) --platform linux/amd64 -t $(docker-registry)$(package):$(monero_version) -t $(docker-registry)$(package):latest --push -f ./monero .

build-monero-exporter:
	$(eval package=monero-exporter)
	sudo docker buildx build --platform linux/amd64 -t $(docker-registry)$(package):latest --push -f ./exporter .

build-monero-nodemapper:
	$(eval package=monero-nodemapper)
	sudo docker buildx build --platform linux/amd64 -t $(docker-registry)$(package):latest --push -f ./nodemapper .
