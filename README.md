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

For any kubectl plugin you want to use do something like this:

```
$ cd
$ git clone https://github.com/dperique/kubectl-plugins.git
$ export PATH=$PATH:kubectl-plugins
$ kubectl config use-context <some-context>

# Tryout the "contstat" plugin.
#
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
$ kubectl contstat somenamespace v

# Check pods, deployments, daemonsets, statefulsets for a namespace
# called "mynamespace".
#
$ kubectl contstat somenamespace
```
