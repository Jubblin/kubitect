[tag 2.0.0]: https://github.com/MusicDin/kubitect/releases/tag/v2.0.0
[tag 2.1.0]: https://github.com/MusicDin/kubitect/releases/tag/v2.1.0

<h1 align="center">Cluster node template</h1>

The note template in the cluster section of the configuration **defines the properties of all nodes** in the cluster.
This includes the properties of the operating system (OS), DNS, and virtual machine user.

## Configuration

### Virtual machnine user

:material-tag-arrow-up-outline: [v2.0.0][tag 2.0.0]
&ensp;
:octicons-file-symlink-file-24: Default: `k8s`

The user property defines the name of the passwordless user created on each virtual machine.
It is used to access the virtual machines during cluster configuration.
If the user property is omitted, a user named `k8s` is created on all virtual machines.
This user can also be used later to access each virtual machine via SSH.

```yaml
cluster:
  nodeTemplate:
    user: kubitect
```

### OS distribution

:material-tag-arrow-up-outline: [v2.1.0][tag 2.1.0]
&ensp;
:octicons-file-symlink-file-24: Default: `ubuntu`

The operating system for virtual machines can be specified in the node template.
Currently, either Ubuntu or Debian can be configured.

By default, the Ubuntu distribution is installed on all virtual machines.
To use Debian instead, set `os.distro` property to Debian.
Images for Ubuntu are obtained from the [Ubuntu cloud image repository](https://cloud-images.ubuntu.com/) and for Debian from the [Debian cloud image repository](https://cloud.debian.org/images/cloud/).

Preset distributions are the following:

+ `ubuntu` - Latest Ubuntu 22.04 release. (*default*)
+ `ubuntu22` - Ubuntu 22.04 release *2022-07-12*.
+ `ubuntu20` - Ubuntu 20.04 release *2022-07-11*.
+ `debian` - Latest Debian 11 release.
+ `debian11` - Debian 11 release *2022-07-11*.



```yaml
cluster:
  nodeTemplate:
    os:
      distro: debian # (1)
```

1. The distribution `ubuntu` is used by default.

To use a custom Ubuntu or Debian image, specify its source.
The source of an image can be either a local path on a system or an URL pointing to the image download.

```yaml
cluster:
  nodeTemplate:
    os:
      distro: ubuntu
      source: https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img
```

### Custom DNS list

:material-tag-arrow-up-outline: [v2.1.0][tag 2.1.0]

The list of Domain Name Servers (DNS) can be configured in the node template.
These servers are used by all virtual machines for DNS resolution.
By default, a DNS list contains only the network gateway.

```yaml
cluster:
  nodeTemplate:
    dns: # (1)
      - 1.1.1.1
      - 1.0.0.1
```

1. IP addresses `1.1.1.1` and `1.0.0.1` represent CloudFlare's primary and secondary public DNS resolvers, respectively.


### SSH options

#### Custom SSH certificate

:material-tag-arrow-up-outline: [v2.0.0][tag 2.0.0]

Kubitect ensures that SSH certificates are automatically generated before the cluster is deployed.
The generated certificates are located in the `config/.ssh/` directory inside a cluster directory.
You can use a custom SSH certificate by specifying a local path to the private key.
Note that the public key must be located in the same directory with the `.pub` suffix.

```yaml
cluster:
  nodeTemplate:
    ssh:
      privateKeyPath: "~/.ssh/id_rsa_test"
```

!!! warning "Warning"

    SSH certificates must be passwordless, otherwise Kubespray will fail to configure the cluster.


#### Adding nodes to the known hosts

:material-tag-arrow-up-outline: [v2.0.0][tag 2.0.0]
&ensp;
:octicons-file-symlink-file-24: Default: `false`

In addition, Kubitect allows you to add all created virtual machines to SSH known hosts on the local machine.
To enable this behavior, set the `addToKnownHosts` property to true.

```yaml
cluster:
  nodeTemplate:
    ssh:
      addToKnownHosts: true
```
