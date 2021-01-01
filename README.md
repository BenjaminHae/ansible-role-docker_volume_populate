# role docker_volume_populate

Using this role allows you to copy data into a docker volume.

Sources can be either archives (`tar.gz`, or similar), local directories or other docker volumes.

## Example

```
import_role:
  name: 'docker_volume_populate'
vars:
  stop_containers_before:
    - seeddms
  type: "archive"
  purge_existing: true
  path: "/tmp/test.tar"
  volume_name: "destination_volume"
```

## Parameters

|--------------------------|--------------------------------------|------------------------------------------------------------------------|
| name                     | default                              | description                                                            |
|--------------------------|--------------------------------------|------------------------------------------------------------------------|
| `stop_containers_before` | `[]`                                 | Names of containers that should be stopped before copying              |
| `start_containers_after` | defaults to `stop_containers_before` | Names of containers that should be stopped after copying               |
| `image`                  | `busybox:latest`                     | Base image to use for the copying container                            |
| `container_name`         | `ansible-copy-volumes-container`     | Name of the copying container                                          |
| `type`                   | `directory`                          | Type of source: `directory`, `archive` or `volume`                     |
| `purge_existing`         | `false`                              | Delete all existing content in destination volume                      |
| `path`                   |                                      | Path of source directory or archive (tar.gz), or name of source volume |
| `volume_name`            |                                      | Name of destination volume                                             |
|--------------------------|--------------------------------------|------------------------------------------------------------------------|
