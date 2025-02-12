# LDE (Local Development Environment) Configuration

This folder contains the configuration files and scripts necessary for setting up and managing a **Local Development Environment (LDE)** using **k3d**. Below is an outline of what this folder contains.

## Contents

1. **k3d Configuration File**
   - [`k3d_default.yml`](k3d-default.yaml): Defines and manages the k3d cluster. Customize cluster settings such as:
      - Number of nodes (servers and agents)
      - Ports mapping for host and cluster
      - Volume mounts
      - Cluster name and runtime options

2. **Python Management Script**
   - [`manage_k3d_cluster.py`](manage_k3d_cluster.py): A Python-based script to streamline the setup, maintenance, and teardown of the cluster.  
     Features:
      - **Start the cluster**: Create and launch the k3d cluster using the `k3d_default.yml` file.
      - **Stop the cluster**: Stop the running k3d cluster.
      - **Delete the cluster**: Delete the k3d cluster.
      - **List clusters**: List all existing k3d clusters.

## Prerequisites

To use the files and script in this folder, ensure that the following tools are installed on your system:

- [k3d](https://k3d.io): A lightweight wrapper to run Kubernetes clusters in Docker.
- [arkade](https://github.com/alexellis/arkade): Portable marketplace for Kubernetes applications.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): Command-line tool for interacting with Kubernetes clusters.
- [Docker](https://www.docker.com/): Required for running k3d clusters.
- [Python 3.6+](https://www.python.org/): Required to execute the management script.

## Usage

### Listing Clusters

To list all existing clusters:
```bash
python manage_k3d_cluster.py list
```
This command will display the names and details of all k3d clusters currently available on the system.


### Setting Up the Cluster

1. Review and, if necessary, modify the cluster configuration in `k3d_default.yml`:
   - Adjust the number of server or agent nodes.
   - Customize node ports and volume definitions.
   - Extend runtime options as required.

2. Use the Python script to create or manage the cluster:

   Run the following command to **start** the cluster.
   ```bash
   python manage_k3d_cluster.py start
   ```

3. Verify that the cluster is running:
   ```bash
   kubectl wait --for=condition=Ready nodes --all --timeout=300s
   ```

### Stopping the Cluster

To stop the cluster:
```bash
python manage_k3d_cluster.py stop
```

### Deleting the Cluster

To delete the cluster:
```bash
python manage_k3d_cluster.py delete
```
remove text after this line

### Checking NVIDIA GPU Availability

To facilitate development with GPU-accelerated workloads, `manage_k3d_cluster.py` includes a command to verify the availability and proper configuration of an NVIDIA GPU in Docker.

To check GPU availability:
```bash
python manage_k3d_cluster.py check-gpu
```

This command reports whether Docker is properly configured to access an NVIDIA GPU and whether the necessary runtimes are installed.

Possible Outputs:
1. 'NVIDIA GPU is available and properly configured in Docker.'
2. 'NVIDIA runtime is not available in Docker. Ensure the NVIDIA Container Toolkit is installed.'
3. 'Unable to use NVIDIA GPU in Docker. Please ensure drivers and NVIDIA Container Toolkit are installed properly.'
### Configuration Details

The cluster is started based on the content of the `k3d-default.yaml` file. This includes:
- Cluster Name: `lde-cluster`
- Server Nodes: 1
- Agent Nodes: 0
- Image: `docker.io/rancher/k3s:v1.30.3-k3s1`
- Docker Registry:
   - Name: `k3d-registry.localhost`
   - Host: `0.0.0.0`
   - Host Port: `5001`
- Port Mappings:
   - **8080 on host → 80 in cluster**
   - **8443 on host → 443 in cluster**
   - **5001 on host → 5001 in cluster** (Docker registry)
   - **27017 on host → 27017 in cluster** (MongoDB)
   - **6379 on host → 6379 in cluster** (Redis)
   - **5432 on host → 5432 in cluster** (PostgreSQL)

### Notes

- The Python script automatically uses the `k3d-default.yaml` configuration file. Ensure it is in the same directory as the script.
- Modify `k3d-default.yaml` as needed to fit your local development setup requirements.
- Use `kubectl` to manage resources within the cluster once it’s running.

- Use the `check-gpu` command in `manage_k3d_cluster.py` to verify NVIDIA GPU availability for workload acceleration.

## References

- [k3d Documentation](https://k3d.io)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
