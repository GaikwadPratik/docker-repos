SHELL:=/bin/bash
VERSION:=$(shell cat .next-version)
TAG:=$(subst +,-,$(VERSION))
IMAGE_NAME:=gaikwadpratik/ubuntu-vscode-golang
SED:=$(shell command -v gsed || command -v sed)
ARCH_TYPE:=$(shell uname -m)
ARCH_VARIANT:= $(shell echo "$(ARCH_TYPE)" | $(SED) -e 's/x86_64/amd64/g' -e 's/i686/i386/g' -e 's/aarch64/arm64/g')

.PHONY: docker-builder
docker-builder:
	docker buildx create \
		--name multiplatform-builder-$(TAG) \
		--platform "linux/amd64,linux/arm64" \
		--driver=docker-container \
		--use \
		--driver-opt=image=moby/buildkit
	docker buildx inspect --bootstrap

.PHONY: docker-build
docker-build: docker-builder
	docker buildx build --force-rm --push --platform linux/amd64,linux/arm64 --no-cache \
		-f Dockerfile \
		-t $(IMAGE_NAME):$(TAG) \
		-t $(IMAGE_NAME):latest \
		.

.PHONY: docker-build-local
docker-build-local: docker-builder
	docker buildx build --force-rm --load --platform linux/$(ARCH_VARIANT) --no-cache \
		-f Dockerfile \
		-t $(IMAGE_NAME):$(TAG) \
		-t $(IMAGE_NAME):latest \
		.

.PHONY: docker-clean
docker-clean:
	docker image remove -f $(IMAGE_NAME):latest $(IMAGE_NAME):$(TAG)
	docker buildx prune --builder multiplatform-builder-$(TAG) -f
	docker buildx rm multiplatform-builder-$(TAG)
	# docker image remove -f moby/buildkit
