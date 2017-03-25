.PHONY: clean data lint requirements sync_data_to_s3 sync_data_from_s3

#################################################################################
# GLOBALS                                                                       #
#################################################################################

BUCKET = {{ cookiecutter.s3_bucket }}
PROJECT_NAME = {{ cookiecutter.repo_name }}
PYTHON_INTERPRETER = {{ cookiecutter.python_interpreter }}


#################################################################################
# FOR CONDA                                                                     #
#################################################################################
ifeq (3,$(findstring 3,$(PYTHON_INTERPRETER)))
PYTHON_VERSION = 3
else
PYTHON_VERSION = 2
endif

ifeq ($(SYSTEM),Linux)
	PLATFORM = Linux
endif
ifeq ($(SYSTEM),Darwin)
	PLATFORM = MacOSX
endif

MINICONDA = Miniconda$(PYTHON_VERSION)-latest-$(PLATFORM)-x86_64.sh

ifeq (,$(shell which conda))
	HAS_CONDA = False
else
	HAS_CONDA = True
endif

environment:
	@echo $(MINICONDA)

#################################################################################
# COMMANDS                                                                      #
#################################################################################

download_conda:
ifeq (True, $(HAS_CONDA))
	@echo ">>> Detected conda. Please run make create_environment"
else
	$(shell sh -c 'wget -P /tmp https://repo.continuum.io/miniconda/$(MINICONDA)')
	$(shell sh -c 'chmod u+x /tmp/$(MINICONDA)')
	@echo "\n>>> Please run /tmp/$(MINICONDA)"
endif

## Set up python interpreter environment from conda
create_environment:
ifeq (True,$(HAS_CONDA))
	$(shell sh -c 'conda config --add channels conda-forge')
	ifeq (3,$(PYTHON_VERSION))
		conda create --name $(PROJECT_NAME) python=3.5
	else
		conda create --name $(PROJECT_NAME) python=2.7
	endif
	@echo ">>> New conda environment created. Activate with:\nsource activate $(PROJECT_NAME)"
else
	@echo ">>> Conda not detected. Please run make download_conda"
endif

## Install Python Dependencies
install_requirements:
ifeq (True,$(HAS_CONDA))
ifeq ($(CONDA_DEFAULT_ENV),$(PROJECT_NAME))
	conda env update -f $(PROJECT_NAME).yml
else
	@echo ">>> You are not in the right environment.\nPlease run source activate "$(PROJECT_NAME)
endif
endif

## Create the data and subfolders
create_data_folders:
	for f in external interim processed raw; do mkdir -p data/$$f ; touch data/$$f/.gitkeep ; done

## Make Dataset
data: requirements
	$(PYTHON_INTERPRETER) src/data/make_dataset.py

## Delete all compiled Python files
clean:
	find . -name "*.pyc" -exec rm {} \;

## Lint using flake8
lint:
	flake8 --exclude=lib/,bin/,docs/conf.py .

## Upload Data to S3
sync_data_to_s3:
	aws s3 sync data/ s3://$(BUCKET)/data/

## Download Data from S3
sync_data_from_s3:
	aws s3 sync s3://$(BUCKET)/data/ data/

## Test python environment is setup correctly
test_environment:
	$(PYTHON_INTERPRETER) test_environment.py

#################################################################################
# PROJECT RULES                                                                 #
#################################################################################



#################################################################################
# Self Documenting Commands                                                     #
#################################################################################

.DEFAULT_GOAL := show-help

# Inspired by <http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html>
# sed script explained:
# /^##/:
# 	* save line in hold space
# 	* purge line
# 	* Loop:
# 		* append newline + line to hold space
# 		* go to next line
# 		* if line starts with doc comment, strip comment character off and loop
# 	* remove target prerequisites
# 	* append hold space (+ newline) to line
# 	* replace newline plus comments by `---`
# 	* print line
# Separate expressions are necessary because labels cannot be delimited by
# semicolon; see <http://stackoverflow.com/a/11799865/1968>
.PHONY: show-help
show-help:
	@echo "$$(tput bold)Available rules:$$(tput sgr0)"
	@echo
	@sed -n -e "/^## / { \
		h; \
		s/.*//; \
		:doc" \
		-e "H; \
		n; \
		s/^## //; \
		t doc" \
		-e "s/:.*//; \
		G; \
		s/\\n## /---/; \
		s/\\n/ /g; \
		p; \
	}" ${MAKEFILE_LIST} \
	| LC_ALL='C' sort --ignore-case \
	| awk -F '---' \
		-v ncol=$$(tput cols) \
		-v indent=19 \
		-v col_on="$$(tput setaf 6)" \
		-v col_off="$$(tput sgr0)" \
	'{ \
		printf "%s%*s%s ", col_on, -indent, $$1, col_off; \
		n = split($$2, words, " "); \
		line_length = ncol - indent; \
		for (i = 1; i <= n; i++) { \
			line_length -= length(words[i]) + 1; \
			if (line_length <= 0) { \
				line_length = ncol - indent - length(words[i]) - 1; \
				printf "\n%*s ", -indent, " "; \
			} \
			printf "%s ", words[i]; \
		} \
		printf "\n"; \
	}' \
	| more $(shell test $(shell uname) == Darwin && echo '--no-init --raw-control-chars')
