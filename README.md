# hello go k8s

Messing around with k8s.

Check the deploy folder for things to play around with

- `./deploy/build-image.sh` creates an image on dockerhub of this app for k8s to pull down and use. If you're forking this, you'll need to log in to dockerhub and change the username parts.
    - Or figure out how to get k8s be able to use a local registry. I couldn't
- `./deploy/web.yaml` contains the desired state for our cluster.
- `./deploy/deploy.sh` will send the yaml to k8s for it to set it all up
- The above will set things up, but it will be in the private docker network. To expose it to localhost, you'll need to run `./deploy/forward-ports.sh`

## learning notes

Some of this could be wrong, just going to update as I go.

### Pods

> A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.

Don't create them directly, create them as part of a deployment.

### Deployment
Describe the shape of your deployment in terms of what kinds of software you want running and how many of them. K8s will maintain the state for you. 

If you change the deployment (say, change the number of replicas) and then re-apply your yaml, k8s will sort it out. It won't kill everything and start again either, it'll just add new pods.

Similarly, if you do `kubectl get pods` and then `kubectl delete pod XXXX`, the pod will get deleted, but the deployment will kick in and create a new one for you. 

### Service

Used to expose an application (a set of pods) as a service. Load balances for you.

# ALSO: Messing around with acceptance tests and having a reference implementation for myself

## Requirements for acceptance tests

- Just describe in terms of the domain. Easier starting point for dev once you've had a discussion with stakeholders.
- Decoupled from implementation detail
- Describe "from the top" want you want in a test without worrying about how
- Be able to run easily locally against
  - A docker container version, with real dependencies (e.g docker compose dependencies)
  - Against a real deployed environment to check its deployed and other infrastrcture concerns are working correctly (terraform, config, secrets, etc etc)
  - Bonus, as a unit test

TODO: Do a vid where I add a new bit of functionality using the approach. Make it _completely_ new, e.g the birthday greeting thing