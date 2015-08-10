https://github.com/bahamas10/node-manta-sync in a Docker image. 
https://hub.docker.com/r/misterbisson/manta-sync

```
docker run -d --restart=always \
-e "TLSCA=`cat "$DOCKER_CERT_PATH"/ca.pem`" \
-e "TLSCERT=`cat "$DOCKER_CERT_PATH"/cert.pem`" \
-e "TLSKEY=`cat "$DOCKER_CERT_PATH"/key.pem`" \
-e "DOCKER_CERT_PATH=/root/.sdc/docker/" \
-e "DOCKER_HOST" \
-e "DOCKER_CLIENT_TIMEOUT=300" \
-e "DOCKER_TLS_VERIFY=1" \
misterbisson/triton-mesos-slave
```

Copied from https://github.com/bahamas10/node-manta-sync :

Usage
-----

    manta-sync ./ ~~/stor/foo

`manta-sync` requires 2 arguments, the first is a local directory that you
would like to sync *the contents* of into manta.  The second is
a manta directory that you would like the files to by synced to.

All remote directories will be lazily created for you if they do not exist,
relying on the latest `manta` node module for this behavior.

If you supply `-r`, `manta-sync` will work in reverse, pulling files from
manta onto your local filesystem.

    manta-sync -r ~~/stor/foo ./foo

Usage

    usage: manta-sync [OPTIONS] localdir ~~/remotedir

    synchronize all files found inside `localdir` to `~~/remotedir`

    examples

      manta-sync ./ ~~/stor/foo
        - sync all files in your cwd to the dir ~~/stor/foo

      manta-sync --dry-run ./ ~~/stor/foo
        - same as above, but just HEAD the data, don't PUT

      manta-sync -r ~~/stor/foo ./bar
        - sync all files from manta in ~~/stor/foo to the local dir ./bar

    options:
        -a ACCOUNT, --account=ACCOUNT       Manta Account (login name). Environment:
                                            MANTA_USER=ACCOUNT
        --user=USER, --subuser=USER         Manta User (login name). Environment:
                                            MANTA_SUBUSER=USER
        --role=ROLE,ROLE,...                Assume a role. Use multiple times or
                                            once with a list. Environment:
                                            MANTA_ROLE=ROLE,ROLE,...
        -h, --help                          Print this help and exit
        -i, --insecure                      Do not validate SSL certificate.
                                            Environment: MANTA_TLS_INSECURE=1
        -k FINGERPRINT, --keyId=FINGERPRINT SSH key fingerprint. Environment:
                                            MANTA_KEY_ID=FINGERPRINT
        -u URL, --url=URL                   Manta URL. Environment: MANTA_URL=URL
        -v, --verbose                       verbose mode
        -c COPIES, --copies=COPIES          number of copies to make
        -d, --delete                        delete files on the remote end not found
                                            locally
        -x ARG, --exclude=ARG               a pattern to ignore when searching the
                                            local filesystem
        -H HEADER, --header=HEADER          HTTP headers to include
        -j, --just-delete                   don't send local files, just delete
                                            extra remote files
        -m, --md5                           use md5 instead of file size (slower,
                                            but more accurate)
        -n, --dry-run                       don't perform any remote PUT or DELETE
                                            operations
        -p CONCURRENCY, --parallel=CONCURRENCY
                                            limit concurrent operations
        -r, --reverse                       manta to local sync
        -U, --updates                       check for available updates on npm
        -V, --version                       print the version number and exit
