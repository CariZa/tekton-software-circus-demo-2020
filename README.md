# tekton-software-circus-demo-2020

Demo scripts and useful links for Tekton discussion at the Software Circus meetup https://www.meetup.com/Software-Circus/events/272202310/ 

### Setup

This requires you to have a cluster setup, you can also run these on minikube.

For cloud clusters please check the installation instructions in the docs:

https://github.com/tektoncd/pipeline/blob/master/docs/install.md

https://github.com/tektoncd/dashboard/blob/master/docs/install.md

For minikube you can just run these (no special permission required):

Apply Pipelines to cluster

    $ kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

Apply Dashboard to cluster

    $ kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml

### 01-task-examples

    task-01.yaml - single step
    task-02.yaml - multiple steps & steps sharing data
    task-03.yaml - git repo example (alpha + beta combo)

Commands:

    $ kubectl apply -f ./01-task-examples

Example 1

```yaml
cat <<EOF | kubectl create -f -
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: 01-task-funbox-demo-$(date +%s)
spec:
  taskRef:
    name: 01-task-funbox-demo
  params:
    - name: PASS_VALUE_INTO_TASK
      value: "Custom value set"
EOF
```

Example 2

```yaml
cat <<EOF | kubectl create -f -
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: 02-task-funbox-demo-with-result-$(date +%s)
spec:
  taskRef:
    name: 02-task-funbox-demo-with-result

EOF
```

Example 3

```yaml
cat <<EOF | kubectl create -f -
apiVersion: tekton.dev/v1alpha1
kind: TaskRun
metadata:
  name: 03-task-git-repo-$(date +%s)
spec:
  taskRef:
    name: 03-task-git-repo
  resources:
    inputs:
      - name: workspace
        resourceRef:
          name: git-empty-hello-image
EOF
```

### 02-pipeline-examples

    01-pipeline.yaml - single task
    02-pipeline.yaml - multiple tasks

Commands:

    $ kubectl apply -f ./02-pipeline-examples

01-pipeline.yaml - single task

```yaml
cat <<EOF | kubectl create -f -
apiVersion: tekton.dev/v1alpha1
kind: PipelineRun
metadata:
  name: 01-demo-git-pipeline-$(date +%s)
spec:
  pipelineRef:
    name: 01-demo-git-pipeline
  resources:
    - name: workspace
      resourceRef:
        name: git-empty-hello-image
EOF
```

02-pipeline.yaml - multiple tasks

```yaml
cat <<EOF | kubectl create -f -
apiVersion: tekton.dev/v1alpha1
kind: PipelineRun
metadata:
  name: 02-demo-multiple-tasks-$(date +%s)
spec:
  pipelineRef:
    name: 02-demo-multiple-tasks
  resources:
    - name: workspace
      resourceRef:
        name: git-empty-hello-image
EOF
```
## Keep going

Here are some examples you can work through on the Tekton Pipelines repo:

https://github.com/tektoncd/pipeline/tree/master/examples

Take a look at Tekton Triggers and see if you can get triggers working:

https://github.com/tektoncd/triggers

https://github.com/tektoncd/triggers/tree/master/examples

I've got some old examples, but from a while ago, so they might be a bit outdated:

https://github.com/CariZa/tekton-examples

I also wrote some stuff to help learn Kaniko

https://github.com/CariZa/kaniko-walkthrough

(It's a work in progress, but hopefully can help a bit with getting started with kaniko)
