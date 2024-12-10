# Run

## Kubectl

```sh
# Create clean local cluster and install Tekton
# You may need to wait a few minutes for Tekton to fully start up all the containers
kind delete cluster ; kind create cluster && kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml && kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml && kubectl create namespace dagger && kubectl config set-context --current --namespace dagger

# Run the PipelineRun
tkn hub install task git-clone ; kubectl delete -f ./templates/git-pipeline-run.yaml ; find ./templates -name '*.yaml' | grep -v 'git-pipeline-run.yaml' | xargs -I {} kubectl apply -f {} && kubectl create -f ./templates/git-pipeline-run.yaml
```

# Details

## Tekton

```sh
# Info
tkn pipelinerun describe

tkn pipelinerun logs my-pipeline-run -f

tkn taskrun logs my-pipeline-run-dagger -fs read

# Debug
tkn taskrun describe

tkn taskrun logs my-pipeline-run-fetch-source -f
```
