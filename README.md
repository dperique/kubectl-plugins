# Example kubectl plugins

Sometimes plain kubectl isn't enough and you end up making scripts that
automate more checks you wish kubectl could do.  Often, those checks are
custom.  [Kubectl plugins](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/)
are useful for implementing things like that but still using the kubectl command.

## Installation and Requirements

First install kubectl. There are many ways, but here's one example:

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.15.4/bin/linux/amd64/kubectl
$ chmod a+x kubectl
$ sudo mv kubectl /usr/local/bin
```

For any kubectl plugin you want to use, do something like this:

Method 1: For those of you with Internet access, do this:

```
$ cd
$ git clone https://github.com/dperique/kubectl-plugins.git
$ export PATH=$PATH:kubectl-plugins
$ kubectl config use-context <some-context>
```

Method 2: For those of you that run things on machines without Internet access,
do this (we assume you have ssh access to your other machine; I will
call it "deployer1"):

```
# On your machine with Internet access, clone the repo:
#
$ cd
$ git clone https://github.com/dperique/kubectl-plugins.git

# scp the files to your "deployer1" machine.
#
$ ssh deployer1 "mkdir kubectl-plugins"
$ scp kubectl-plugins/* deployer1:kubectl-plugins

# From your "deployer1" machine, set your path.
#
$ ssh deployer1
$ export PATH=$PATH:kubectl-plugins
$ kubectl config use-context <some-context>
```

Tryout the "contstat" plugin.

```
$ kubectl contstat help

  Usage:

    kubectl contstat <aNamespace> [<verbose>]

    We use the current context (dp-test)
    Checks Ready status of Pods in Deployments, Daemonsets, and Statefulsets
    and checks for Pods not in Complete or Running state

    kubectl constat mynamespace v ;# will use verbose mode
```

## kubectl contstat plugin

This one goes out and checks the status of Pods and commonly used Kubernetes
in my Kubernetes clusters.

Examples:

```
# Get help text.
#
$ kubectl help

# Check pods, deployments, daemonsets, statefulsets for a namespace
# called "mynamespace" in verbose mode.
#
$ kubectl contstat mynamespace v

# Check pods, deployments, daemonsets, statefulsets for a namespace
# called "mynamespace".
#
$ kubectl contstat mynamespace
```
