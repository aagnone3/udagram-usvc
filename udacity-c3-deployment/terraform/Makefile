# variables
TF_VARS_FILE ?= variables.tfvars.json
TF_PLAN_FILE ?= terraform_plan.txt
TF_OUTPUT_FILE ?= tf.json
TF_CMD ?= terraform
KUBE_CLUSTER_NAME ?= $(shell cat $(TF_VARS_FILE) | jq -r '.cluster_name')
K1_CMD ?= kubeone
K1_CFG_FILE ?= kubeone-config.yml

# definitions
KUBE_CONFIG_FILE := $(KUBE_CLUSTER_NAME)-kubeconfig
KUBECONFIG=$(PWD)/$(KUBE_CONFIG_FILE)

init:
	$(TF_CMD) init

plan:
	$(TF_CMD) plan -var-file $(TF_VARS_FILE) -out $(TF_PLAN_FILE)

apply:
	$(TF_CMD) apply $(TF_PLAN_FILE)

output:
	$(TF_CMD) output -json > $(TF_OUTPUT_FILE)

cluster:
	$(K1_CMD) install $(K1_CFG_FILE) --tfjson .
	echo Recommended: export KUBECONFIG=$$PWD/$(KUBE_CONFIG_FILE)

destroy:
	$(K1_CMD) reset $(K1_CFG_FILE) --tfjson .
	$(TF_CMD) destroy
	
deploy_secrets:
	kubectl apply -f ../k8s/aws-secret.yaml
	kubectl apply -f ../k8s/env-secret.yaml
	kubectl apply -f ../k8s/env-configmap.yaml

deploy_pods:
	kubectl apply -f ../k8s/backend-user-deployment.yaml
	kubectl apply -f ../k8s/backend-feed-deployment.yaml
	kubectl apply -f ../k8s/reverseproxy-deployment.yaml
	kubectl apply -f ../k8s/frontend-deployment.yaml

deploy_services:
	kubectl apply -f ../k8s/backend-user-service.yaml
	kubectl apply -f ../k8s/backend-feed-service.yaml
	kubectl apply -f ../k8s/frontend-service.yaml
	kubectl apply -f ../k8s/reverseproxy-service.yaml