ZIP=zip
GIT_FOLDER=.git

GIT_HOOKS=$(GIT_FOLDER)/hooks
GIT_HOOKS_FILES=$(GIT_HOOKS)/applypatch-msg
GIT_HOOKS_FILES+=$(GIT_HOOKS)/commit-msg
GIT_HOOKS_FILES+=$(GIT_HOOKS)/pre-applypatch
GIT_HOOKS_FILES+=$(GIT_HOOKS)/pre-commit
GIT_HOOKS_FILES+=$(GIT_HOOKS)/pre-push
GIT_HOOKS_FILES+=$(GIT_HOOKS)/pre-rebase
GIT_HOOKS_FILES+=$(GIT_HOOKS)/pre-receive
GIT_HOOKS_FILES+=$(GIT_HOOKS)/prepare-commit-msg
GIT_HOOKS_FILES+=$(GIT_HOOKS)/post-update
GIT_HOOKS_FILES+=$(GIT_HOOKS)/update

SCRIPT_SOURCE_FOLDER=scripts
SCRIPT_FOLDER=scripts/git-hooks

SCRIPT_LIST=$(SCRIPT_FOLDER)/pre-commit.mention-at-start
SCRIPT_LIST+=$(SCRIPT_FOLDER)/pre-commit.npm-no-local-package
SCRIPT_LIST+=$(SCRIPT_FOLDER)/commit-msg.prepend-branch-identifier

ZIP_TARGETS=$(GIT_FOLDER)
ZIP_TARGETS+=$(SCRIPT_FOLDER)

OUTPUT_FILE=git.zip

all: build

.PHONY: all build clean purge

$(GIT_HOOKS):
	mkdir -p "$(GIT_HOOKS)"
	echo "*" > $(GIT_HOOKS)/.gitignore

$(GIT_HOOKS)/%: hook-redirect $(GIT_HOOKS)
	cp $< $@

$(SCRIPT_FOLDER):
	mkdir -p "$(SCRIPT_FOLDER)"
	[[ "$(SCRIPT_SOURCE_FOLDER)" == "$(SCRIPT_FOLDER)" ]] || echo "*" > $(SCRIPT_FOLDER)/.gitignore

$(SCRIPT_FOLDER)/%: $(SCRIPT_SOURCE_FOLDER)/% $(SCRIPT_FOLDER)
	@# Handle circular dependancy if SCRIPT_FOLDER == SCRIPT_SOURCE_FOLDER
	[[ "$<" == "$(SCRIPT_FOLDER)" ]] || cp "$<" "$@"

$(OUTPUT_FILE): $(GIT_HOOKS_FILES) $(SCRIPT_LIST)
	@rm -f $(OUTPUT_FILE)
	$(ZIP) -r "$(OUTPUT_FILE)" $(ZIP_TARGETS)

build: $(OUTPUT_FILE)

clean:
	rm -rf $(GIT_HOOKS)
	[[ "$(SCRIPT_SOURCE_FOLDER)" == "$(SCRIPT_FOLDER)" ]] || rm -rf $(SCRIPT_FOLDER)

purge: clean
	rm -rf $(OUTPUT_FILE) $(GIT_FOLDER)

