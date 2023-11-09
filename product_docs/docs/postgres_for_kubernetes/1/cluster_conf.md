# Instance pod configuration

## Projected volumes

EDB Postgres for Kubernetes supports mounting custom files inside the Postgres pods through
`.spec.projectedVolumeTemplate`. This ability is useful for several Postgres
features and extensions that require additional data files.
In EDB Postgres for Kubernetes, the `.spec.projectedVolumeTemplate` field is a
[projected volume](https://kubernetes.io/docs/concepts/storage/projected-volumes/)
template in Kubernetes that allows you to mount arbitrary data under the
`/projected` folder in Postgres pods.

This simple example shows how to mount an existing TLS secret (named
`sample-secret`) as files into Postgres pods. The values for the secret keys
`tls.crt` and `tls.key` in `sample-secret` are mounted as files into the  paths
`/projected/certificate/tls.crt` and `/projected/certificate/tls.key` in the
Postgres pod.

```yaml
apiVersion: postgresql.k8s.enterprisedb.io/v1
kind: Cluster
metadata:
  name: cluster-example-projected-volumes
spec:
  instances: 3
  projectedVolumeTemplate:
    sources:
      - secret:
          name: sample-secret
          items:
            - key: tls.crt
              path: certificate/tls.crt
            - key: tls.key
              path: certificate/tls.key
  storage:
    size: 1Gi
```

You can find a complete example that uses a projected volume template to mount
the secret and ConfigMap in the
[cluster-example-projected-volume.yaml](samples/cluster-example-projected-volume.yaml)
deployment manifest.

## Ephemeral volumes

EDB Postgres for Kubernetes relies on [ephemeral volumes](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/)
for part of the internal activities. Ephemeral volumes exist for the sole
duration of a pod's life, without persisting across pod restarts.

### Volume for temporary storage

An ephemeral volume used for temporary storage. You can configure an upper
bound on the size using the `spec.ephemeralVolumesSizeLimit.temporaryData`
field in the cluster spec.

### Volume for shared memory

This volume is used as shared memory space for Postgres and as an ephemeral
type but stored in memory. You can configure an upper bound on the size using
the `spec.ephemeralVolumesSizeLimit.shm` field in the cluster spec.
Use this field only in case of
[PostgreSQL running with `posix` shared memory dynamic allocation](postgresql_conf.md#dynamic-shared-memory-settings).

## Environment variables

You can customize some system behavior using environment variables. One example
is the `LDAPCONF` variable, which can point to a custom LDAP configuration
file. Another example is the `TZ` environment variable, which represents the
timezone used by the PostgreSQL container.

EDB Postgres for Kubernetes allows you to set custom environment variables using the `env`
and the `envFrom` stanza of the cluster specification.

This example defines a PostgreSQL cluster using the `Australia/Sydney`
timezone as the default cluster-level timezone:

```yaml
apiVersion: postgresql.k8s.enterprisedb.io/v1
kind: Cluster
metadata:
  name: cluster-example
spec:
  instances: 3

  env:
  - name: TZ
    value: Australia/Sydney

  storage:
    size: 1Gi
```

The `envFrom` stanza can refer to ConfigMaps or secrets to use their content as
environment variables:

```yaml
apiVersion: postgresql.k8s.enterprisedb.io/v1
kind: Cluster
metadata:
  name: cluster-example
spec:
  instances: 3

  envFrom:
  - configMapRef:
      name: config-map-name
  - secretRef:
      name: secret-name

  storage:
    size: 1Gi
```

The operator doesn't allow setting the following environment variables:

- `POD_NAME`
- `NAMESPACE`
- Any environment variable whose name starts with `PG`.

Any change in the `env` or in the `envFrom` section triggers a rolling
update of the PostgreSQL pods.

If the `env` or the `envFrom` section refers to a secret or a ConfigMap, the
operator doesn't detect any changes in them and doesn't trigger a rollout. The
kubelet uses the same behavior with pods, and you must trigger the pod rollout
manually.