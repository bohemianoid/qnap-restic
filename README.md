# restic on QNAP NAS
> Backups to Backblaze B2 Cloud Storage done right on a QNAP NAS

## Install

### Entware
Install Entware on the QNAP NAS ([instructions](https://github.com/Entware/Entware/wiki/Install-on-QNAP-NAS)).

### Packages
Install required packages using `opkg`.

```sh
$ opkg update
$ opkg install git git-http make
```

### restic
SSH into your QNAP NAS as `admin`, download the latest native [`restic` binary](https://github.com/restic/restic/releases/latest) and decompress it.

```sh
$ wget -qO- https://github.com/restic/restic/releases/download/v0.9.6/restic_0.9.6_linux_amd64.bz2 | bunzip2 > restic
```

Then move the decompressed `restic` binary to `/opt/bin` and make it executable.

```sh
$ mv restic /opt/bin; chmod +x /opt/bin/restic
```

`restic` can now be used from the command line.

```sh
$ restic -h
```

The official `restic` binaries can be updated in place.

```sh
$ restic self-update
```

## Setup

### [erikw/restic-systemd-automatic-backup](https://github.com/erikw/restic-systemd-automatic-backup)
Set up `restic` backups to Backblaze B2 Cloud Storage  ([instructions](https://github.com/erikw/restic-systemd-automatic-backup), steps 1 to 5).

### Crontab
Add a new task to `crontab` and restart the daemon to backup automatically with `restic`.

```sh
$ echo "0 0 * * * /usr/local/sbin/restic_backup.sh" >> /etc/config/crontab
$ crontab /etc/config/crontab && /etc/init.d/crond.sh restart
```

## Use
To use `restic` with Backblaze B2 Cloud Storage, just source the corresponding environment.

```sh
$ source /etc/restic/b2_env.sh
$ restic snapshots
```

## Contributors
* [David Burkardt](https://www.cyon.ch/ueber-uns/team#db)
* [Simon Roth](https://github.com/simonroth)
