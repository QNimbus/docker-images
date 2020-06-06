IMAGE := ${DOCKER_IMAGE_ID}
VERSION:= $(shell grep 'ARG VERSION=' Dockerfile | awk 'NR==1{print $2}' | cut -d '=' -f 2)

test:
	true

image:
	docker build -t ${IMAGE}:${VERSION} .
	docker tag ${IMAGE}:${VERSION} ${IMAGE}:latest

push-image:
	docker push ${IMAGE}:${VERSION}
	docker push ${IMAGE}:latest

.PHONY: image push-image test