IMAGE := ${DOCKER_UBUNTU_ANSIBLE_IMAGE_ID}
VERSION := $(shell grep 'ARG VERSION=' Dockerfile | awk 'NR==1{print $2}' | cut -d '=' -f 2)

test:
	$(eval CONTAINER_ID := $(shell docker run -it --rm --detach ${IMAGE}:${VERSION}))
	docker exec --tty ${CONTAINER_ID} lsb_release -a
	docker stop ${CONTAINER_ID}

image:
	docker build -t ${IMAGE}:${VERSION} .
	docker tag ${IMAGE}:${VERSION} ${IMAGE}:latest

push-image:
	docker push ${IMAGE}:${VERSION}
	docker push ${IMAGE}:latest

.PHONY: image push-image test