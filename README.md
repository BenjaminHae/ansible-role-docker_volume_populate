# role docker_volume_populate

Using this role allows you to copy data into a docker volume, or writing data out of a docer volume.

Sources/Destinations can be either archives (`tar.gz`, or similar), local directories or other docker volumes.

## Examples

### Copying data to volume

```
import_role:
  name: 'docker_volume_populate'
vars:
  stop_containers_before:
    - seeddms
  type: archive
  direction: in
  purge_existing: true
  src: "/tmp/test.tar"
  volume_name: "destination_volume"
```

### Copying data out of volume

```
import_role:
  name: 'docker_volume_populate'
vars:
  stop_containers_before:
    - seeddms
  type: archive
  direction: out
  dest: "/tmp/test.tar"
  volume_name: "source_volume"
```

## Parameters

| name                     | default                              | description                                                                                           |
|--------------------------|--------------------------------------|-------------------------------------------------------------------------------------------------------|
| `stop_containers_before` | `[]`                                 | Names of containers that should be stopped before copying                                             |
| `start_containers_after` | defaults to `stop_containers_before` | Names of containers that should be stopped after copying                                              |
| `image`                  | `busybox:latest`                     | Base image to use for the copying container                                                           |
| `container_name`         | `ansible-copy-volumes-container`     | Name of the copying container                                                                         |
| `type`                   | `directory`                          | Type of source/destination: `directory`, `archive` or `volume`                                        |
| `purge_existing`         | `false`                              | Delete all existing content in destination volume (only for direction=`in`)                           |
| `owner`                  |                                      | Owner that is set for all files written to the destination container (only for `direction=in`)        |
| `src`                    |                                      | Path of source directory or archive (tar.gz), or name of source volume (only for `direction=in`)      |
| `dest`                   |                                      | Path of destination directory or archive (tar.gz), or name of destination volume  for `direction=out` |
| `volume_name`            |                                      | Name of destination or source volume                                                                  |

## How it works

Basically it just spins up a `busybox` container with `src` and `dest` mounted as volumes. In this container a `cp` or `tar` command is executed.
