DOCKER=docker
CURDIR=$(shell pwd)
CONTAINER=app
BUILD_CONTAINER=$(CONTAINER)_build
RELEASE_CONTAINER=$(CONTAINER)_release
PORT=4000
IP=$(shell ipconfig getifaddr en0)

shell:
	$(DOCKER) run -it -p $(PORT):$(PORT) -v "$(CURDIR)":/code $(BUILD_CONTAINER) /bin/sh
.PHONY: shell

iex:
	$(DOCKER) run -it -p $(PORT):$(PORT) -e DISPLAY=$(IP):0 -v /tmp/.X11-unix:/tmp/.X11-unix -v "$(CURDIR)":/code $(BUILD_CONTAINER) iex -S mix
.PHONY: iex

serve:
	$(DOCKER) run -it -p $(PORT):$(PORT) -e DISPLAY=$(IP):0 -v /tmp/.X11-unix:/tmp/.X11-unix -v "$(CURDIR)":/code $(BUILD_CONTAINER) mix phx.server
.PHONY: serve

test:
	$(DOCKER) run -it -p $(PORT):$(PORT) -e DISPLAY=$(IP):0 -v /tmp/.X11-unix:/tmp/.X11-unix -v "$(CURDIR)":/code $(BUILD_CONTAINER) mix test
.PHONY: test

build:
	$(DOCKER) build -t $(BUILD_CONTAINER) -f Dockerfile.build .
.PHONY: build

build_release:
	$(DOCKER) run -it -p $(PORT):$(PORT) -e DISPLAY=$(IP):0 -v /tmp/.X11-unix:/tmp/.X11-unix -v "$(CURDIR)":/code $(BUILD_CONTAINER) mix release
	$(DOCKER) build -t $(RELEASE_CONTAINER) -f Dockerfile.release .
.PHONY: build_release

clean:
	$(DOCKER) rmi -f $(BUILD_CONTAINER)
	$(DOCKER) rmi -f $(RELEASE_CONTAINER)
	rm -rf node_modules/
	rm -rf deps/
	rm -rf _build/
.PHONY: clean
