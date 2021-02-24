---
template: overrides/main.html
title: Install iter8
---

=== "iter8 for Knative"
    Follow these steps to install iter8 for KNative. 


    !!! example "Prerequisites"
        1. Kubernetes cluster with [Knative Serving](https://knative.dev/docs/install/any-kubernetes-cluster/#installing-the-serving-component)
        2. [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
        3. [Kustomize v3](https://kubectl.docs.kubernetes.io/installation/kustomize/), and 
        4. [Go 1.13+](https://golang.org/doc/install)

    ## Step 1: Clone repo
    ```shell
    git clone https://github.com/iter8-tools/iter8.git
    cd iter8
    export ITER8=$(pwd)
    ```

    ## Step 2: Install iter8-monitoring
    ```shell
    kustomize build $ITER8/install/monitoring/prometheus-operator | kubectl apply -f -
    kubectl wait crd -l creator=iter8 --for condition=established --timeout=120s
    kustomize build $ITER8/install/monitoring/prometheus | kubectl apply -f - 
    ```

    ## Step 3: Install iter8
    ```shell
    kustomize build $ITER8/install | kubectl apply -f -
    kubectl wait crd -l creator=iter8 --for condition=established --timeout=120s
    kustomize build $ITER8/install/iter8-metrics | kubectl apply -f -
    ```

    ## Step 4: Install iter8ctl
    ```shell
    GO111MODULE=on GOBIN=/usr/local/bin go get github.com/iter8-tools/iter8ctl@v0.1.0-pre
    ```

=== "iter8 for KFServing"
    An initial version of iter8 for KFServing is available [here](https://github.com/iter8-tools/iter8-kfserving) along with installation instructions. An updated version is coming soon and will be documented here.

=== "iter8 for Istio"
    An earlier version of iter8 for Istio is available [here](https://github.com/iter8-tools/iter8) along with installation instructions. An updated version is coming soon and will be documented here.