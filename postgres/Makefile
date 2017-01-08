NAME = strues/postgres
VERSION = 9.6

.PHONY: all build run clean tag_latest clean_images

all: build

build:
	@docker build \
	 -t $(NAME):$(VERSION) --rm .

run:
	rm -rf /tmp/postgresql
	mkdir -p /tmp/postgresql

	@docker run -d -t \
		-e POSTGRES_DB=boldr \
		-e POSTGRES_USER=boldr \
		-e POSTGRES_PASSWORD=password \
		-p 5432:5432 \
		-v ./volumes/pg:/var/lib/postgresql/data \
		-e DEBUG=true \
		--name postgresql -t $(NAME):$(VERSION)


clean:
	docker exec -t postgresql /bin/bash -c "sv stop postgresql" || true
	sleep 2
	docker exec -t postgresql /bin/bash -c "rm -rf /etc/postgresql/9.6/main/*" || true
	docker exec -t postgresql /bin/bash -c "rm -rf /var/lib/postgresql/9.6s/main/*" || true
	docker stop postgresql postgresql_no_postgresql postgresql_default || true
	docker rm postgresql postgresql_no_postgresql postgresql_default || true
	rm -rf /tmp/postgresql || true

tag_latest:
	docker tag $(NAME):$(VERSION) $(NAME):latest

clean_images:
	docker rmi $(NAME):latest $(NAME):$(VERSION) || true
