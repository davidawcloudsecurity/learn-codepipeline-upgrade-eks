# learn-codepipeline-upgrade-eks
how to upgrade eks
```bash
#!/bin/bash

# Define versions
cluster_version=1.22
eks_version=1.24

# Convert versions to numbers for comparison
cluster_version_num=$(echo $cluster_version | awk -F. '{print $1*100 + $2}')
eks_version_num=$(echo $eks_version | awk -F. '{print $1*100 + $2}')

# Check if cluster_version is lower than eks_version
if (( $(echo "$cluster_version_num < $eks_version_num" | bc -l) )); then
    echo "do nothing"
else
    # Update eks_version by incrementing until it equals cluster_version
    while (( $(echo "$eks_version_num < $cluster_version_num" | bc -l) )); do
        echo "Require update: "
        eks_version_num=$(echo "$eks_version_num + 1" | bc)
        eks_version=$(echo "scale=2; $eks_version_num / 100" | bc)
        echo "cluster_version: $cluster_version"
        echo "eks_version: $eks_version"
        read -p "Press any key to continue..."
    done
    echo "Updated eks_version to $eks_version"
fi
```
how to restart deploy
```bash
#!/bin/bash

# Define the namespace
NAMESPACE="kube-system"

# Get all deployments in the specified namespace
DEPLOYMENTS=$(kubectl get deployments -n $NAMESPACE -o jsonpath='{.items[*].metadata.name}')

# Loop through each deployment and restart it
for DEPLOYMENT in $DEPLOYMENTS; do
  echo "Rolling out deployment: $DEPLOYMENT"
  kubectl rollout restart deploy $DEPLOYMENT -n $NAMESPACE
done

echo "All deployments in the namespace $NAMESPACE have been restarted."
```
how to restart pod
```bash
#!/bin/bash

# Define the namespace
NAMESPACE="kube-system"

# Get all pods in the specified namespace
PODS=$(kubectl get pods -n $NAMESPACE -o jsonpath='{.items[*].metadata.name}')

# Loop through each pod and delete it
for POD in $PODS; do
  echo "Deleting pod: $POD"
  kubectl delete pod $POD -n $NAMESPACE
done

echo "All pods in the namespace $NAMESPACE have been deleted."
```
