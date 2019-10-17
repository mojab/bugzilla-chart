# BUGZILLA

Bugzilla
========

Bugzilla is free and open source web-based bug-tracking software that is
developed by an active group of volunteers in the Mozilla community, and used
by thousands of projects and companies around the world. It can be installed on
Linux and other flavors of Unix, Windows or Mac OS X.

You can try Bugzilla out using our testing installation:
https://landfill.bugzilla.org/bugzilla-tip/

Documentation
=============

Bugzilla's comprehensive documentation, including installation instructions,
can be found here:
http://www.bugzilla.org/docs/


## Introduction

This chart bootstraps a [Bugzilla](https://github.com/bugzilla/bugzilla) deployment on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

It also packages the [MariaDB chart](https://github.com/kubernetes/charts/tree/master/stable/mariadb) which is required for bootstrapping a MariaDB deployment for the database requirements of the Bugzilla application.

Charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters.

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure


## Configuration

The following table lists the configurable parameters of the Bugzilla chart and their default values.

|            Parameter             |                Description                 |                         Default                         |
|----------------------------------|--------------------------------------------|---------------------------------------------------------|
| `image.registry`                 | Bugzilla image registry                    | `docker.io`                                             |
| `image.repository`               | Bugzilla image name                        | `nasqueron/bugzilla`                                    |
| `image.tag`                      | Bugzilla image tag                         | `latest`                                                |
| `image.pullPolicy`               | Image pull policy                          | `Always` if `imageTag` is `latest`, else `IfNotPresent` |
| `image.pullSecrets`              | Specify image pull secrets                 | `nil`                                                   |
| `bugzillaUrl`                    | Short URLs site refering to the app        | `http://bugzilla`                                       |
| `bugzillaAdminEmail`             | User of the application                    | `admin@localhost`                                       |
| `bugzillaAdminPassword`          | Application password                       | _random 10 character long alphanumeric string_          |
| `extraEnv`                       | Additional environment variables           | `[]`                                                    |
| `replicaCount`                   | Number of Bugzilla Pods to run               | `1`                                                     |
| `mariadb.enabled`                | Deploy MariaDB container(s)                | `true`                                                  |
| `mariadb.rootUser.password`      | MariaDB admin password                     | `nil`                                                   |
| `mariadb.db.name`                | Database name to create                    | `bugzilla`                                              |
| `mariadb.db.user`                | Database user to create                    | `bugzilla`                                              |
| `mariadb.db.password`            | Password for the database                  | _random 10 character long alphanumeric string_          |
| `mariadb.service.port`           | Port for the database                      | `3306`                                                  |
| `externalDatabase.host`          | Host of the external database              | `localhost:3306`                                        |
| `externalDatabase.user`          | Existing username in the external db       | `bugzilla`                                              |
| `externalDatabase.password`      | Password for the above username            | `nil`                                                   |
| `externalDatabase.database`      | Name of the existing database              | `bugzilla`                                              |
| `service.annotations`            | Service annotations                        | `{}`                                                    |
| `service.type`                   | Kubernetes Service type                    | `LoadBalancer`                                          |
| `service.loadBalancerIP`         | Load balancer IP                           | `LoadBalancer`                                          |
| `service.clusterIP`              | Cluster IP                                 | `LoadBalancer`                                          |
| `service.port`                   | Service HTTP port                          | `80`                                                    |
| `service.httpsPort`              | Service HTTPS port                         | `443`                                                   |
| `service.externalTrafficPolicy`  | Enable client source IP preservation       | `Cluster`                                               |
| `nodePorts.http`                 | Kubernetes http node port                  | `""`                                                    |
| `nodePorts.https`                | Kubernetes https node port                 | `""`                                                    |
| `ingress.enabled`                | Enable ingress controller resource         | `false`                                                 |
| `ingress.certManager`            | Add annotations for cert-manager           | `false`                                                 |
| `ingress.annotations`            | Ingress annotations                        | `[]`                                                    |
| `ingress.hosts[0].name`          | Hostname to your Bugzilla installation     | `bugzilla.local`                                        |
| `ingress.hosts[0].path`          | Path within the url structure              | `/`                                                     |
| `ingress.hosts[0].tls`           | Utilize TLS backend in ingress             | `false`                                                 |
| `ingress.hosts[0].tlsSecret`     | TLS Secret (certificates)                  | `bugzilla.local-tls-secret`                             |
| `ingress.secrets[0].name`        | TLS Secret Name                            | `nil`                                                   |
| `ingress.secrets[0].certificate` | TLS Secret Certificate                     | `nil`                                                   |
| `ingress.secrets[0].key`         | TLS Secret Key                             | `nil`                                                   |
| `persistence.enabled`            | Enable persistence using PVC               | `true`                                                  |
| `persistence.existingClaim`      | Enable persistence using an existing PVC   | `nil`                                                   |
| `persistence.storageClass`       | PVC Storage Class                          | `nil` (uses alpha storage class annotation)             |
| `persistence.accessMode`         | PVC Access Mode                            | `ReadWriteOnce`                                         |
| `persistence.size`               | PVC Storage Request                        | `10Gi`                                                  |
| `volumes`                        | Additional volumes                         | `nil`                                                   |
| `volumeMounts`                   | Additional volumeMounts                    | `nil`                                                   |
| `nodeSelector`                   | Node labels for pod assignment             | `{}`                                                    |
| `tolerations`                    | List of node taints to tolerate            | `[]`                                                    |
| `affinity`                       | Map of node/pod affinities                 | `{}`                                                    |
| `podAnnotations`                 | Pod annotations                                  | `{}`                                                         |
| `metrics.enabled`                | Start a side-car prometheus exporter             | `false`                                                      |
| `metrics.image.registry`         | Apache exporter image registry                   | `docker.io`                                                  |
| `metrics.image.repository`       | Apache exporter image name                       | `lusotycoon/apache-exporter`                                 |
| `metrics.image.tag`              | Apache exporter image tag                        | `v0.5.0`                                                     |
| `metrics.image.pullPolicy`       | Image pull policy                                | `IfNotPresent`                                               |
| `metrics.image.pullSecrets`      | Specify docker-registry secret names as an array | `nil`                                                        |
| `metrics.podAnnotations`         | Additional annotations for Metrics exporter pod  | `{prometheus.io/scrape: "true", prometheus.io/port: "9117"}` |
| `metrics.resources`              | Exporter resource requests/limit                 | {}                                                           |

The above parameters map to the env variables defined in [Bugzilla](https://hub.docker.com/r/nasqueron/bugzilla). For more information please refer to the [Bugzilla](https://hub.docker.com/r/nasqueron/bugzilla) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set bugzillaAdminEmail=admin,bugzillaAdminPassword=password,mariadb.mariadbRootPassword=secretpassword \
    mojab/bugzilla
```

The above command sets the Bugzilla administrator account username and password to `admin` and `password` respectively. Additionally, it sets the MariaDB `root` user password to `secretpassword`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml mojab/bugzilla
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [Bugzilla](https://hub.docker.com/r/nasqueron/bugzilla) image stores the Bugzilla data and configurations at the `/var/www/html` path of the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.

## Using an external database

Sometimes you may want to have Bugzilla connect to an external database rather than installing one inside your cluster, e.g. to use a managed database service, or use run a single database server for all your applications. To do this, the chart allows you to specify credentials for an external database under the [`externalDatabase` parameter](#configuration). You should also disable the MariaDB installation with the `mariadb.enabled` option. For example:

```console
$ helm install mojab/bugzilla \
    --set mariadb.enabled=false,externalDatabase.host=myexternalhost:3306,externalDatabase.user=myuser,externalDatabase.password=mypassword,externalDatabase.database=mydatabase
```

Note also if you disable MariaDB per above you MUST supply values for the `externalDatabase` connection.

## Ingress

This chart provides support for ingress resources. If you have an
ingress controller installed on your cluster, such as [nginx-ingress](https://kubeapps.com/charts/stable/nginx-ingress)
or [traefik](https://kubeapps.com/charts/stable/traefik) you can utilize
the ingress controller to serve your Bugzilla application.

To enable ingress integration, please set `ingress.enabled` to `true`

### Hosts

Most likely you will only want to have one hostname that maps to this
Bugzilla installation, however, it is possible to have more than one
host.  To facilitate this, the `ingress.hosts` object is an array.

For each item, please indicate a `name`, `tls`, `tlsSecret`, and any
`annotations` that you may want the ingress controller to know about.

Indicating TLS will cause Bugzilla to generate HTTPS URLs, and
Bugzilla will be connected to at port 443.  The actual secret that
`tlsSecret` references do not have to be generated by this chart.
However, please note that if TLS is enabled, the ingress record will not
work until this secret exists.

For annotations, please see [this document](https://github.com/kubernetes/ingress-nginx/blob/master/docs/annotations.md).
Not all annotations are supported by all ingress controllers, but this
document does a good job of indicating which annotation is supported by
many popular ingress controllers.

### TLS Secrets

This chart will facilitate the creation of TLS secrets for use with the
ingress controller, however, this is not required.  There are three
common use cases:

* helm generates/manages certificate secrets
* user generates/manages certificates separately
* an additional tool (like [kube-lego](https://kubeapps.com/charts/stable/kube-lego))
manages the secrets for the application

In the first two cases, one will need a certificate and a key.  We would
expect them to look like this:

* certificate files should look like (and there can be more than one
certificate if there is a certificate chain)

```
-----BEGIN CERTIFICATE-----
MIID6TCCAtGgAwIBAgIJAIaCwivkeB5EMA0GCSqGSIb3DQEBCwUAMFYxCzAJBgNV
...
jScrvkiBO65F46KioCL9h5tDvomdU1aqpI/CBzhvZn1c0ZTf87tGQR8NK7v7
-----END CERTIFICATE-----
```
* keys should look like:
```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvLYcyu8f3skuRyUgeeNpeDvYBCDcgq+LsWap6zbX5f8oLqp4
...
wrj2wDbCDCFmfqnSJ+dKI3vFLlEz44sAV8jX/kd4Y6ZTQhlLbYc=
-----END RSA PRIVATE KEY-----
````

If you are going to use Helm to manage the certificates, please copy
these values into the `certificate` and `key` values for a given
`ingress.secrets` entry.

If you are going to manage TLS secrets outside of Helm, please
know that you can create a TLS secret by doing the following:

```
kubectl create secret tls bugzilla.local-tls --key /path/to/key.key --cert /path/to/cert.crt
```

Please see [this example](https://github.com/kubernetes/contrib/tree/master/ingress/controllers/nginx/examples/tls)
for more information.
