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
