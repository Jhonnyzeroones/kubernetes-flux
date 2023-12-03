# kubernetes-flux
Demo app of a kubernetes deployment on flux.

Cluster creation:

    * The cluster is created through terraform in EKS.
    * This needs a valid AWS account and AWS cli configred.
    * Once the cluster is created the kubeconfig file needs to be generated.
    * It can be generated using:
        aws eks --region $(terraform output -raw region) update-kubeconfig \
        --name $(terraform output -raw cluster_name)

Kubernetes deployment:

    * The kubernetes deployment needs the flux installed locally.
    * Follow the following steps to bootstrap flux: https://fluxcd.io/flux/installation/#gitlab-and-gitlab-enterprise
    * The flux bootstrap is performed direcly after the cluster has booted. The infra and app deployments will be delivered with flux.
    * Flux command for the deployment: 

        flux bootstrap github \
        --context="1" \
        --owner=${GITHUB_USER} \
        --repository=${GITHUB_REPO} \
        --branch=main \
        --personal \
        --path=clusters/k8s-test-cluster

    * context here is the output of the command "kubectl config get-contexts"

    * After the loadbalancer has been deployed, DNS has to be pointed to the loadbalancer.
