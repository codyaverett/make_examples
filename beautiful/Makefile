# Ref: https://8thlight.com/insights/make-developing-easier-by-building-a-beautiful-makefile

# This is a regular comment, that will not be displayed

.DEFAULT_GOAL := help2

.PHONY: help help2 help3

yo:
	@echo "yo yo yo"

## -----
## This is a help comment
## -----

help: ## Show this help.
	@sed -ne '/@sed/!s/## //p' $(MAKEFILE_LIST)

help2: ## help2 is cool with awk
		@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n make \033[36m\033[0m\n"} /^[$$()% 0-9a-zA-Z_-]+:.*?##/ { printf " \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)

# COLORS
GREEN := $(shell tput -Txterm setaf 2)
YELLOW := $(shell tput -Txterm setaf 3)
WHITE := $(shell tput -Txterm setaf 7)
RESET := $(shell tput -Txterm sgr0)

TARGET_MAX_CHAR_NUM=20
## Show help
help3: ## the third help is the best help
	@echo ''
	@echo 'Usage:'
	@echo ' $(YELLOW)make$(RESET) $(GREEN)<target>$(RESET)'
	@echo ''
	@echo 'Targets:'
	@awk '/^[a-zA-Z\-\_0-9]+:/' {
		helpMessage = match(lastLine, /^## (.*)/);
		if (helpMessage) {
			helpCommand = substr($$1, 0, index($$1, ":")-1);
			helpMessage = substr(lastLine, RSTART + 3, RLENGTH);
			printf " $(YELLOW)%-$(TARGET_MAX_CHAR_NUM)s$(RESET) $(GREEN)%s$(RESET)\n", helpCommand, helpMessage;
		} 
	}
	{ lastLine = $$0 }' $(MAKEFILE_LIST)
