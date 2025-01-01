IMG ?= opennebula4test
CONTAINER ?= opennebula-test
CONTAINER_START_TIMEOUT ?= 10

# CONTAINER_TOOL defines the container tool to be used for building images.
# Be aware that the target commands are only tested with Docker which is
# scaffolded by default. However, you might want to replace it to use other
# tools. (i.e. podman)
CONTAINER_TOOL ?= docker

# Setting SHELL to bash allows bash commands to be executed by recipes.
# Options are set to exit when a recipe line exits non-zero or a piped command fails.
SHELL = /usr/bin/env bash -o pipefail
.SHELLFLAGS = -ec

##@ General

# The help target prints out all targets with their descriptions organized
# beneath their categories. The categories are represented by '##@' and the
# target descriptions by '##'. The awk commands is responsible for reading the
# entire set of makefiles included in this invocation, looking for lines of the
# file as xyz: ## something, and then pretty-format the target and help. Then,
# if there's a line with ##@ something, that gets pretty-printed as a category.
# More info on the usage of ANSI control characters for terminal formatting:
# https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_parameters
# More info on the awk command:
# http://linuxcommand.org/lc3_adv_awk.php

.PHONY: help
help: ## Display this help.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

##@ Development

.PHONY: test
test: ## Run tests.
	ONE_AUTH=$(PWD)/one_auth go test -C ./src/goca -v

.PHONY: test-with-container
test-with-container: test-image-build remove-test-container ## Run tests within OpenNebula container image.
	$(CONTAINER_TOOL) run -d --name ${CONTAINER} -p 9869:9869 -p 2633:2633 -p 2474:2474 -p 2616:2616 ${IMG} /bin/bash -c "one start; sunstone-server start; oneflow-server start; onegate-server start; fireedge-server start; sleep infinity"
	curl --head -X GET --retry ${CONTAINER_START_TIMEOUT} --retry-all-errors --retry-delay 1 localhost:2633
	curl --head -X GET --retry ${CONTAINER_START_TIMEOUT} --retry-all-errors --retry-delay 1 localhost:2474
	make test || make remove-test-container

.PHONY: remove-test-container
remove-test-container: ## Remove running OpenNebula test image.
	$(CONTAINER_TOOL) rm ${CONTAINER} --force

.PHONY: test-image-build
test-image-build: ## Build container image with the OpenNebula binaries for tests.
	$(CONTAINER_TOOL) build -t ${IMG} .
