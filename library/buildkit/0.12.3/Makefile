# This file is generated by the template.

REGISTRY	?=lcr.loongnix.cn
ORGANIZATION	?=library
REPOSITORY	?=buildkit
TAG		?=0.12.3-debian
LATEST		?=true

IMAGE=$(REGISTRY)/$(ORGANIZATION)/$(REPOSITORY):$(TAG)
LATEST_IMAGE=$(REGISTRY)/$(ORGANIZATION)/$(REPOSITORY):latest
GIT_URL=https://github.com/Loongson-Cloud-Community/buildkit.git

default: image

src:
	git clone -b loong64-0.12.3 $(GIT_URL) $@

image: src
	cd src/ && docker buildx build -t lcr.loongnix.cn/library/buildkit:0.12.3-debian -f Dockerfile   --build-arg https_proxy=$(https_proxy) --build-arg BUILDPLATFORM=linux/loong64 .
	#cd src/ && docker buildx build -t lcr.loongnix.cn/library/buildkit:0.12.3 -f Dockerfile.static .
push:
	cd src/ && docker buildx build --load -t lcr.loongnix.cn/library/buildkit:0.12.3-debian -f Dockerfile . && docker push lcr.loongnix.cn/library/buildkit:0.12.3-debian
	#cd src/ && docker buildx build --load -t lcr.loongnix.cn/library/buildkit:0.12.3 -f Dockerfile.static . && docker push lcr.loongnix.cn/library/buildkit:0.12.3

clean:
	rm -rf src
