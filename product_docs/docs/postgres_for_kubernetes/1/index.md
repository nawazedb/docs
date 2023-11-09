# EDB Postgres for Kubernetes

The EDB Postgres for Kubernetes operator is a fork based on [CloudNativePG](https://cloudnative-pg.io).
It provides additional value such as compatibility with Oracle using EDB
Postgres Advanced Server and additional supported platforms such as IBM Power
and OpenShift. It is designed, developed, and supported by EDB and covers the
full lifecycle of a highly available Postgres database clusters with a
primary/standby architecture, using native streaming replication.

!!! Note
    The operator has been renamed from Cloud Native PostgreSQL. Existing users
    of Cloud Native PostgreSQL will not experience any change, as the underlying
    components and resources have not changed.

## Key features in common with CloudNativePG

- Kubernetes API integration for high availability
  - CloudNativePG uses the `postgresql.cnpg.io/v1` API version
  - EDB Postgres for Kubernetes uses the `postgresql.k8s.enterprisedb.io/v1` API version
- Self-healing through failover and automated recreation of replicas
- Capacity management with scale up/down capabilities
- Planned switchovers for scheduled maintenance
- Read-only and read-write Kubernetes services definitions
- Rolling updates for Postgres minor versions and operator upgrades
- Continuous backup and point-in-time recovery
- Connection Pooling with PgBouncer
- Integrated metrics exporter out of the box
- PostgreSQL replication across multiple Kubernetes clusters
- Separate volume for WAL files

## Features unique to EDB Postgres of Kubernetes

- [Long Term Support](#long-term-support)
- Red Hat certified operator for OpenShift
- Support on IBM Power and z/Linux through partnership with IBM
- [Oracle compatibility](https://www.enterprisedb.com/docs/epas/latest/fundamentals/epas_fundamentals/epas_compat_ora_dev_guide/) through EDB Postgres Advanced Sever
- [Transparent Data Encryption (TDE)](https://www.enterprisedb.com/docs/tde/latest/) through EDB Postgres Advanced Server
- EDB Postgres for Kubernetes Plugin
- Velero/OADP cold backup support
- Generic adapter for third-party Kubernetes backup tools

You can [evaluate EDB Postgres for Kubernetes for free](evaluation.md).
You need a valid license key to use EDB Postgres for Kubernetes in production.

!!! Note
    Based on the [Operator Capability Levels model](operator_capability_levels.md),
    users can expect a **"Level V - Auto Pilot"** set of capabilities from the
    EDB Postgres for Kubernetes Operator.

### Long Term Support

EDB is committed to declaring a Long Term Support (LTS) version of EDB 
Postgres for Kubernetes annually (1.18 was our first). Each LTS version will 
receive maintenance releases and be supported for an additional 12 months beyond 
the last community release of CloudNativePG for the same version. 

For example, the last version of 1.18 of CloudNativePG was released on June 12, 2023.  
Because this was declared an LTS version of EDB Postgres for Kubernetes, it will be supported
for additional 12 months until June 12, 2024.  

In addition, customers will always have at least 6 months to move between LTS versions. This
means a new LTS version will be available by January 12, 2024 at the latest. 

While we encourage customers to regularly upgrade to the latest version of the operator to take
advantage of new features, having LTS versions allows customers desiring additional stability to stay on the same
version for 12-18 months before upgrading.

## Licensing

EDB Postgres for Kubernetes works with both PostgreSQL and EDB Postgres
Advanced server, and is available under the
[EDB Limited Use License](https://www.enterprisedb.com/limited-use-license).

You can [evaluate EDB Postgres for Kubernetes for free](evaluation.md).
You need a valid license key to use EDB Postgres for Kubernetes in production.

## Supported releases and Kubernetes distributions

For a list of the minor releases of EDB Postgres for Kubernetes that are
supported by EDB, please refer to the
["Platform Compatibility"](https://www.enterprisedb.com/resources/platform-compatibility#pgk8s)
page. Here you can also find which Kubernetes distributions and versions are
supported for each of them and the EOL dates.

### Multiple architectures

The EDB Postgres for Kubernetes Operator container images support the multi-arch
format for the following platforms: `linux/amd64`, `linux/arm64`, `linux/ppc64le`, `linux/s390x`.

!!! Warning
    EDB Postgres for Kubernetes requires that all nodes in a Kubernetes cluster have the
    same CPU architecture, thus a hybrid CPU architecture Kubernetes cluster is not
    supported. Additionally, EDB supports `linux/ppc64le` and `linux/s390x` architectures
    on OpenShift only.

## Supported Postgres versions

The following versions of Postgres are currently supported:

- PostgreSQL 15, 14, 13, 12, and 11
- EDB Postgres Advanced 15, 14, 13, 12, and 11

All of the above versions are available on the following platforms: `linux/amd64`, `linux/arm64`, `linux/ppc64le`, `linux/s390x`.
EDB supports operand images for `linux/ppc64le` and `linux/s390x` architectures
on OpenShift only.

## About this guide

Follow the instructions in the ["Quickstart"](quickstart.md) to test EDB Postgres for Kubernetes
on a local Kubernetes cluster using Kind, or Minikube.

In case you are not familiar with some basic terminology on Kubernetes and PostgreSQL,
please consult the ["Before you start" section](before_you_start.md).

!!! Note
    Although the guide primarily addresses Kubernetes, all concepts can
    be extended to OpenShift as well.

*[Postgres, PostgreSQL and the Slonik Logo](https://www.postgresql.org/about/policies/trademarks/)
are trademarks or registered trademarks of the PostgreSQL Community Association
of Canada, and used with their permission.*