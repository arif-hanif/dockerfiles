IMAGE = strues/alpine
TAG = 3.5-sup

clean:
	docker rmi $(IMAGE):$(TAG) $(CLEAN_OPTIONS) || true

build:
	docker build -t $(IMAGE):$(TAG) $(BUILD_OPTIONS) .

pull:
	docker pull $(IMAGE):$(TAG) $(PULL_OPTIONS)

push:
	docker push $(IMAGE):$(TAG)
