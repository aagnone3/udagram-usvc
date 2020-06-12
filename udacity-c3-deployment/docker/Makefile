include ../Makefile.registry

# variables
AWS_ACCOUNT_ID ?= 528600220408
AWS_REGION ?= us-east-1
AWS_CMD ?= aws
DOCKER_CMD ?= docker
IMAGE_NAME ?= hello-eks
CLUSTER_NAME ?= $(IMAGE_NAME)

# derived variables
ECR_BASE := $(AWS_ACCOUNT_ID).dkr.ecr.$(AWS_REGION).amazonaws.com
ECR_URI := $(ECR_BASE)/$(IMAGE_NAME)

kubeconfig:
	$(AWS_CMD) eks --profile default --region $(AWS_REGION) update-kubeconfig --name $(CLUSTER_NAME)

image:
	$(DOCKER_CMD) build -t $(IMAGE_NAME) .