---
title: 'Restore Keycloak'
date: '2024-01-08T13:26:23-05:00'
categories: ["how-to"]
description: How to restore a Keycloak backup on Rhize
weight: 300
menu:
  main:
    parent: restore
    identifier:
---

This guide shows you how to restore Keycloak in your Rhize environment.

| Restoring Keycloak to an existing running instance will involve downtime

Expected downtime for an existing running instance depends on the cluster performance sizing, backup size and bandwidth speed to the kubernetes cluster. Downtime is typically under a minute.

## Prerequisites

Before you start, ensure you have the following:

- [`kubectl`](https://kubernetes.io/docs/tasks/tools/)
- A [Keycloak backup]({{< relref "../backup/keycloak" >}})

## Steps

1. Confirm the cluster and namespace are correct:

    {{% param "k8s_cluster_ns" %}}

1. Retrieve the Keycloak user password using the following command, replacing <namespace> with your namespace:

  ```bash
  kubectl get secret keycloak-<namespace>-postgresql -o jsonpath="{.data.postgres-password}" | base64 --decode
  ```

1. Extract your backup file 

  ```bash
  gzip -d keycloak-postgres-backup-YYYYMMDDTHHMMAA.sql
  ```

1. Scale down the replicas of Keycloak to 0 to prevent new records being created whilst the restore is in progress. Keycloak will be unavailable after this command

  ```bash
  kubectl scale statefulsets keycloak --replicas=0
  ```

1. Scale down the replicas of PostgreSQL to 0, so that existing persistent volume claims and persistent volumes can be removed.

  ```bash
  kubectl scale statefulsets keycloak-postgresql --replicas=0
  ```

1. Remove the Postgres persistent volume claim

  ```bash
  kubectl delete pvc data-keycloak-postgresql-0
  ```

1. Identify the Keycloak Postgres volume(s):

  ```bash
  kubectl get pv | grep keycloak
  ```

  It will display a list of persistent volume claims related to Keycloak. For example:
  ```
  pvc-95176bc4-88f4-4178-83ab-ee7b256991bc   10Gi       RWO            Delete           Terminating   libre/data-keycloak-postgresql-0   hostpath                48d
  ```

  Note that `pvc-*` items will be named different, note the pvc names.

1. Remove the persistent volume(s), by issuing the command replacing <pvc-from-previous-step> with your `pvc-*` from the previous step.

  ```
  $ kubectl delete pv <pvc-from-previous-step>
  ```

1. Scale up the replicas of PostgreSQL to 1.

  ```bash
  kubectl scale statefulsets keycloak-postgresql --replicas=1
  ```

1. Restore the backup
     
     ```bash
     cat keycloak-postgres-backup-YYYYMMDDTHHMMAA.sql | kubectl exec -i keycloak-postgresql-0 -- psql postgresql://postgres:<your-postgres-password>@localhost:5432 -U postgres
     ```

1. Scale up the replicas of Keycloak to 1.

  ```bash
  kubectl scale statefulsets keycloak --replicas=1
  ```

1. Proxy the web portal of Keycloak and confirm access by issuing the command and checking `http://localhost:80`

  ```bash
  kubectl port-forward  svc/keycloak 5101:80
  ```