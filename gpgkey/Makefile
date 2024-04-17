# Makefile to manage GPG keys including Git integration for commit signing

# Default GPG key details (can be overridden)
GPG_KEY_TYPE ?= default
GPG_KEY_LENGTH ?= 2048
GPG_SUBKEY_TYPE ?= default
GPG_SUBKEY_LENGTH ?= 2048
GPG_EXPIRE_DATE ?= 2y

# Backup directory
BACKUP_DIR ?= ./gpg-backups

.PHONY: help generate-key list-keys backup-keys restore-keys git-sign-config amend-sign-commits

help:
	@echo "Makefile for managing GPG keys"
	@echo "  generate-key       - Generates a new GPG key. You will be prompted for your name and email."
	@echo "  list-keys          - Lists existing GPG keys."
	@echo "  backup-keys        - Backups GPG keys and the trust database."
	@echo "  restore-keys       - Restores GPG keys and the trust database from backups."
	@echo "  git-sign-config    - Configures git to sign commits with a GPG key, use KEY=<key-id>."
	@echo "  amend-sign-commits - Amends all commits to be signed with a GPG key, use KEY=<key-id>."

generate-key:
	@read -p "Enter your name: " GPG_NAME_REAL; \
	read -p "Enter your email: " GPG_NAME_EMAIL; \
	echo Generating GPG key for $$GPG_NAME_REAL \<$$GPG_NAME_EMAIL\>; \
	gpg --batch --generate-key <(echo "Key-Type: $(GPG_KEY_TYPE)\nKey-Length: $(GPG_KEY_LENGTH)\nSubkey-Type: $(GPG_SUBKEY_TYPE)\nSubkey-Length: $(GPG_SUBKEY_LENGTH)\nName-Real: $$GPG_NAME_REAL\nName-Email: $$GPG_NAME_EMAIL\nExpire-Date: $(GPG_EXPIRE_DATE)\n%no-protection\n%commit")

list-keys:
	@gpg --list-secret-keys --keyid-format=long

backup-keys:
	@mkdir -p $(BACKUP_DIR)
	@echo Backing up GPG keys and trust database...
	gpg --export-secret-keys > $(BACKUP_DIR)/secret-keys.gpg
	gpg --export-ownertrust > $(BACKUP_DIR)/ownertrust.gpg
	@echo Backup completed.

restore-keys:
	@echo Restoring GPG keys and trust database from backup...
	gpg --import $(BACKUP_DIR)/secret-keys.gpg
	gpg --import-ownertrust $(BACKUP_DIR)/ownertrust.gpg
	@echo Restore completed.

git-sign-config:
ifndef KEY
	$(error KEY is undefined. Usage: make git-sign-config KEY=<key-id>)
endif
	@git config --global user.signingkey $(KEY)
	@git config --global commit.gpgsign true
	@echo "Git configured to sign commits using GPG key $(KEY)"

amend-sign-commits:
ifndef KEY
	$(error KEY is undefined. Usage: make amend-sign-commits KEY=<key-id>)
endif
	@git filter-branch --commit-filter 'git commit-tree -S$$1 $$@;' HEAD
	@echo "All commits have been amended to be signed with key $(KEY)"
