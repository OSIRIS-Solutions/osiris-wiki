VERSION = $(shell git describe --tags --abbrev=0)

deploy:
	if [ -z "$(VERSION)" ]; then \
		echo "No version tag found. Please create a version tag before deploying."; \
		exit 1; \
	fi
	mike deploy $(VERSION) latest --update-aliases
	@echo "Deployed version $(VERSION) as latest"