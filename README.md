# Ghost service for Kubernetes on Wodby

Run Ghost as a reusable, persistent application service with Wodby.

This repository defines the Wodby service manifest and operational contract for Ghost.

- [Ghost service on Wodby](https://wodby.com/services/ghost)
- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [Ghost application stack](https://github.com/wodby/stack-ghost)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `ghost` |
| Type | Application service |
| Versions | Ghost 6 |
| Workloads | `main` (StatefulSet), primary, one replica |
| Containers | `ghost` using the official `ghost` image |
| Endpoint | HTTP 2368 with a 50 MiB request-body limit |
| Links | MySQL 8 database and SMTP relay |
| Volume | Content, 10 GB by default |
| Operations | Content import and backup |
| Helm | `oci://registry-1.docker.io/wodby/stateful`, version `0.2.0` |

## Runtime requirements

Ghost requires MySQL 8 in production; MariaDB is not supported. Transactional email is sent through the linked SMTP
relay. Set the required email sender to an address accepted by that relay.

Ghost 6 stores persistent files in `/var/lib/ghost/content`. Do not change this service to Ghost 7 as a compatible
image update: Ghost 7 uses a different content path and requires an explicit storage migration.

## Maintain a custom version

1. Fork this repository.
2. Edit `service.yml`.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference `ghost` from a stack manifest.

Keep service, workload, container, endpoint, link, volume, and setting names stable unless consuming stacks and
app-level overrides are updated at the same time.

Validate the manifest with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [managed services index](https://github.com/wodby/services) for other Wodby services.
