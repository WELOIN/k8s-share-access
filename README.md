# k8s-share-access

A simple, interactive CLI tool to share Kubernetes cluster access with team members. Create limited-access kubeconfig files without sharing your admin credentials.

## Features

- **Interactive UI** - Arrow key navigation, checkboxes, and colored output
- **Multi-context support** - Switch between different Kubernetes clusters
- **Granular permissions** - Read-only or Read-write access per namespace
- **Cluster-wide options** - Optional access to nodes, storage, and cluster events
- **User management** - List, create, edit, and delete users with confirmations
- **Port forwarding** - Optional port-forward permission for debugging
- **Secure kubeconfig** - Generates kubeconfig with namespace-scoped contexts
- **Cross-platform** - Works on macOS and Linux (no external dependencies)

## Installation

### Quick Install

```bash
# Download and install globally
curl -sL https://raw.githubusercontent.com/YOUR_USERNAME/k8s-share-access/main/k8s-share-access -o k8s-share-access
chmod +x k8s-share-access
./k8s-share-access --install
```

### Manual Install

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/k8s-share-access.git
cd k8s-share-access

# Make executable and install
chmod +x k8s-share-access
./k8s-share-access --install
```

## Usage

Simply run the script to start the interactive mode:

```bash
k8s-share-access
```

### Main Menu

After selecting a Kubernetes context, you'll see the main menu:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   Kubernetes Access Sharing Tool
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Context: my-cluster

What would you like to do?
  ❯ List users
    Create user
    Edit user
    Delete user
    Change context
    Exit
```

### Creating a User

1. **Enter username** - ServiceAccount name for the new user
2. **Select access level** - Read-only or Read-write
3. **Select namespaces** - All namespaces or specific ones
4. **Cluster access** - Optional access to nodes, storage, events
5. **Port forwarding** - Optional port-forward capability
6. **Token expiry** - Choose token lifetime (24h to never expires)
7. **Confirm** - Review summary and apply

The tool generates a kubeconfig file (e.g., `kubeconfig-username.yaml`) that can be shared securely.

### Editing a User

1. **Select user** - Pick from existing users
2. **Modify settings** - Change access level, namespaces, cluster access, or port forwarding
3. **Review changes** - Summary shows what changed with `(changed)` indicators
4. **Apply** - Updates permissions without regenerating credentials

### Permissions

**Read-only access includes:**
- Pods, logs, services, endpoints
- ConfigMaps, Secrets, Events
- Deployments, ReplicaSets, StatefulSets, DaemonSets
- Jobs, CronJobs
- Ingresses, NetworkPolicies
- HorizontalPodAutoscalers
- PersistentVolumeClaims

**Read-write access adds:**
- Create, update, patch, delete operations
- Pod exec capability

**Optional namespace permissions:**
- Port forwarding (`pods/portforward`)

**Optional cluster access:**
- View nodes (cluster health)
- View storage (PersistentVolumes, StorageClasses)
- View cluster events

## Generated Kubeconfig

The generated kubeconfig contains a single context with accessible namespaces listed in comments:

```yaml
apiVersion: v1
kind: Config
#
# Accessible namespaces:
#   - production
#   - staging
#
# Usage:
#   kubectl get pods -n <namespace>
#   kubectl config set-context --current --namespace=<namespace>
#
contexts:
- name: username@cluster
  context:
    cluster: my-cluster
    user: username
```

Users can list available namespaces and switch between them:
```bash
# List all namespaces (user can only access their permitted ones)
kubectl get namespaces

# Access resources in a specific namespace
kubectl get pods -n production

# Set default namespace for current context
kubectl config set-context --current --namespace=staging
```

## Requirements

- `kubectl` installed and configured
- Access to a Kubernetes cluster with admin privileges
- Bash 3.2+ (default on macOS and Linux)

## How It Works

1. Creates a dedicated `shared-access` namespace (if it doesn't exist)
2. Creates a ServiceAccount in the `shared-access` namespace
3. Creates Roles with specified permissions in each selected namespace
4. Creates RoleBindings to link the ServiceAccount to the Roles
5. Creates ClusterRole with namespace listing + optional cluster-wide access
6. Generates a token and creates a kubeconfig file

## License

MIT License - Feel free to use and modify.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.
