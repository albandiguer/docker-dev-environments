ACCOUNT := albandiguer
IMAGE_NAME := $(ACCOUNT)/ruby-dev
PLATFORMS := linux/amd64,linux/arm64
TAG=$(shell cat version.txt)

default: build

build:
	docker buildx build  \
		--push \
		--platform $(PLATFORMS) \
		-t $(IMAGE_NAME):$(TAG) \
		-t $(IMAGE_NAME):latest \
		.

publish:
	docker login -u $(ACCOUNT)
	docker push $(IMAGE_NAME):$(TAG)
