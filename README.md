# CICD pipeline using Tekton

Tekton is a Kubernetes-native continuous integration and delivery (CI/CD) framework
that allows you to create containerized, composable, and configurable workloads 
declaratively through Kubernetes CRDs. When integrated with Tekton Triggers, Tekton
allows you to easily create fully fledged CI/CD systems in which you define all 
mechanics exclusively using Kubernetes resources

This repo details the following two pipelines
* Pipeline to build, deploy, test and promote the cars api app
* Setup a GitHub webhook to trigger pipeline on Github PR (pull request) actions

## Build, deploy, test and promote pipeline
The pipline does the following using Tekto
* Create a local docker registry
* Build the https://github.com/santoshpatil81/cars-api repo and create a docker image
* Push the docker image to the local docker registry
* Deploy the app to DEV environment
* Run Integration tests on the DEV environment
* Promote the changes to STAGING environent


## Github webhook pipeline
Triggers listens for a git commit or a git pull request event. When it detects one, it 
executes an integration test to validate the committed code.

## Pre-Requisites:

* Tekton (Cloud Native CI/CD framework)
* Kubernetes (Container Orchestrator framework)
* Docker (Containerization tool)
* Kaniko (Container images builder from Dockerfile)
* Helm (Kubernetes application package manager)
* kind (Kubernetes in Docker framework)

## Setup

* Clone `tekton-kickstarter` repo (https://github.com/MartinHeinz/tekton-kickstarter)
* Populate variables in .env
* Run `make secrets` and verify generated secret values in `./misc/secrets.yaml`
* Run `make` to deploy core components for tekton
* Deploy tekton triggers and pipeline CRDs `$ kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml`
* Deploy tekton dashboard `kubectl apply -f https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml`
* Run the below port-forward command and access tekton dashboard via the link https://localhost:9097 
  `kubectl port-forward svc/tekton-dashboard -n tekton-pipelines 9097`

