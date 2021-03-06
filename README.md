# Scaleway CLI

[![Build Status (Travis)](https://travis-ci.org/moul/scaleway-cli.svg?branch=master)](https://travis-ci.org/moul/scaleway-cli)
[![Dependency Status](https://david-dm.org/moul/scaleway-cli.svg?theme=shields.io)](https://david-dm.org/moul/scaleway-cli)
[![Total views](https://sourcegraph.com/api/repos/github.com/moul/scaleway-cli/counters/views.svg)](https://sourcegraph.com/github.com/moul/scaleway-cli)
[![Views in the last 24 hours](https://sourcegraph.com/api/repos/github.com/moul/scaleway-cli/counters/views-24h.svg)](https://sourcegraph.com/github.com/moul/scaleway-cli)

[![NPM Badge](https://nodei.co/npm/scaleway-cli.png)](https://npmjs.org/package/scaleway-cli)

Interact with Scaleway API from the command line.

Uses [node-scaleway](https://github.com/moul/node-scaleway) SDK.

Maintained by [Manfred Touron](https://github.com/moul)


Usage
-----

Usage 100% inspired by Docker

    $ scw

      Usage: scw [options] [command]


      Commands:

        attach <server>                 attach (serial console) to a running server
        build <path>                    build an image from a file
        commit <server>                 create a new image from a server's changes
        cp <server:path> <path>         copy files/folders from a server's filesystem to the host path
        create <image>                  create a new server but do not start it
        events                          get real time events from the API
        exec <server> <command>         run a command in a running server
        export <server>                 stream the contents of a server as a tar archive
        history <image>                 show the history of an image
        images                          list images
        import                          create a new filesystem image from the contents of a tarball
        info                            display system-wide information
        inspect <item> [otherItems...]  return low-level information on a server or image
        kill <server>                   kill a running server
        load                            load an image from a tar archive
        login                           login to the API
        logout                          log out from the API
        logs <server>                   fetch the logs of a server
        port                            list port security for the server
        pause                           pause all processes within a server
        ps                              list servers
        pull <image>                    pull an image or a repository
        push <image>                    push an image or a repository
        rename <server>                 rename an existing server
        restart <server>                restart a running server
        rm <server>                     remove one or more servers
        rmi <image>                     remove one or more images
        run <image>                     run a command in a new server
        save <image>                    save an image to a tar archive
        search <keyword>                search for an image on the Hub
        start <server>                  start a stopped server
        stop <server>                   stop a running server
        tag <image> <tag>               tag an image into a repository
        top <server>                    lookup the running processes of a server
        unpause <server>                unpause a paused server
        version                         show the version information
        wait <server>                   block until a server stops

      Options:

        -h, --help            output usage information
        -V, --version         output the version number
        --api-endpoint <url>  set the API endpoint
        -D, --debug           enable debug mode


Examples
--------

Create a server with Ubuntu Trusty image and 3.2.34 bootscript

    $ scw create trusty --bootscript=3.2.34
    df271f73-60ce-47fd-bd7b-37b5f698d8b2

Create a server with Fedora 21 image

    $ scw create 1f164079
    7313af22-62bf-4df1-9dc2-c4ffb4cb2d83

Create a server with an empty disc of 20G and rescue bootscript

    $ scw create 20G --bootscript=rescue
    5cf8058e-a0df-4fc3-a772-8d44e6daf582

Run a stopped server

    $ scw start 7313af22
    7313af22-62bf-4df1-9dc2-c4ffb4cb2d83

Wait for a server to be in 'stopped' state

    $ scw wait 7313af22
    [...] some seconds later
    0

Attach to server serial port

    $ scw attach 7313af22
    [RET]
    Ubuntu Vivid Vervet (development branch) nfs-server ttyS0
    my-server login:
    ^C
    $

Create a server with Fedora 21 image and start it

    $ scw start `scw create 1f164079`
    5cf8058e-a0df-4fc3-a772-8d44e6daf582

Execute a 'ls -la' on a server (via SSH)

    $ scw exec myserver -- ls -la
    total 40
    drwx------.  4 root root 4096 Mar 26 05:56 .
    drwxr-xr-x. 18 root root 4096 Mar 26 05:56 ..
    -rw-r--r--.  1 root root   18 Jun  8  2014 .bash_logout
    -rw-r--r--.  1 root root  176 Jun  8  2014 .bash_profile
    -rw-r--r--.  1 root root  176 Jun  8  2014 .bashrc
    -rw-r--r--.  1 root root  100 Jun  8  2014 .cshrc
    drwxr-----.  3 root root 4096 Mar 16 06:31 .pki
    -rw-rw-r--.  1 root root 1240 Mar 12 08:16 .s3cfg.sample
    drwx------.  2 root root 4096 Mar 26 05:56 .ssh
    -rw-r--r--.  1 root root  129 Jun  8  2014 .tcshrc

Run a shell on a server (via SSH)

    $ scw exec 5cf8058e /bin/bash
    [root@noname ~]#

List public images and my images

    $ scw images
    REPOSITORY                                 TAG      IMAGE ID   CREATED        VIRTUAL SIZE
    user/Alpine_Linux_3_1                      latest   854eef72   10 days ago    50 GB
    Debian_Wheezy_7_8                          latest   cd66fa55   2 months ago   20 GB
    Ubuntu_Utopic_14_10                        latest   1a702a4e   4 months ago   20 GB
    ...

List public images, my images and my snapshots

    $ scw images -a
    REPOSITORY                                 TAG      IMAGE ID   CREATED        VIRTUAL SIZE
    noname-snapshot                            <none>   54df92d1   a minute ago   50 GB
    cool-snapshot                              <none>   0dbbc64c   11 hours ago   20 GB
    user/Alpine_Linux_3_1                      latest   854eef72   10 days ago    50 GB
    Debian_Wheezy_7_8                          latest   cd66fa55   2 months ago   20 GB
    Ubuntu_Utopic_14_10                        latest   1a702a4e   4 months ago   20 GB

List running servers

    $ scw ps
    SERVER ID   IMAGE                       COMMAND   CREATED          STATUS    PORTS   NAME
    7313af22    user/Alpine_Linux_3_1                 13 minutes ago   running           noname
    32070fa4    Ubuntu_Utopic_14_10                   36 minutes ago   running           labs-8fe556

List all servers

    $ scw ps -a
    SERVER ID   IMAGE                       COMMAND   CREATED          STATUS    PORTS   NAME
    7313af22    user/Alpine_Linux_3_1                 13 minutes ago   running           noname
    32070fa4    Ubuntu_Utopic_14_10                   36 minutes ago   running           labs-8fe556
    7fc76a15    Ubuntu_Utopic_14_10                   11 hours ago     stopped           backup

Stop a running server

    $ scw stop 5cf8058e
    5cf8058e

Create a snapshot of the root volume of a server

    $ scw commit 5cf8058e
    54df92d1

Delete a stopped server

    $ scw rm 5cf8

Create a snapshot of nbd1

    $ scw commit 5cf8058e -v 1
    f1851f99

Create an image based on a snapshot

    $ scw tag 87f4526b my_image
    46689419

Delete an image

    $ scw rmi 46689419

Send a 'halt' command via SSH

    $ scw kill 5cf8058e
    5cf8058e

Inspect a server

    $ scw inspect 90074de6
    [
      {
        "server": {
        "dynamic_ip_required": true,
        "name": "My server",
        "modification_date": "2015-03-26T09:01:07.691774+00:00",
        "tags": [
          "web",
          "production"
        ],
        "state_detail": "booted",
        "public_ip": {
          "dynamic": true,
          "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "address": "212.47.xxx.yyy"
        },
        "state": "running",
      }
    ]

Show public ip address of a server

    $ scw inspect 90074de6 -f '.server.public_ip.address'
    212.47.xxx.yyy


Workflows
---------

For more examples, see [./examples/](https://github.com/moul/scaleway-cli/tree/master/examples) directory

    # create a server with a nbd1 volume of 50G and rescue bootscript
    $ SERVER=$(scw create trusty --bootscript=rescue --volume=50000000000 --sync)
    # print the ip address of the server
    $ echo "Your server is ready and is available at: $(scw inspect ${SERVER} -f .server.public_ip.address)"

Debug
-----

`scaleway-cli` uses the [debug](https://www.npmjs.com/package/debug) package.

To enable debug you can use the environment variable `DEBUG=` as :

- `DEBUG='*' scw ...` to see debug for `scaleway-cli` and all dependencies
- `DEBUG='scaleway-cli:*' scw ...` to see debug for `scaleway-cli`
- `DEBUG='node-scaleway:*' scw ...` to see debug for `node-scaleway`

    $ DEBUG='*' scw images
      node-scaleway:lib GET https://api.cloud.online.net/images? +0ms { method: 'GET',
      url: 'https://api.cloud.online.net/images?',
      headers:
       { Accept: 'application/json',
         'User-Agent': 'node-scaleway',
         'X-Auth-Token': 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' },
      resolveWithFullResponse: true,
      json: true }
    REPOSITORY                                 TAG      IMAGE ID   CREATED        VIRTUAL SIZE
    Fedora_21_Twenty-one                       latest   1f164079   10 days ago    50 GB
    user/Archlinux_latest                      latest   1197ca91   10 days ago    50 GB
    ...
    scaleway-cli:utils saveEntities: removed 15 items +0ms
    scaleway-cli:utils saveEntities: inserted 15 items +4ms


Install
-------

1. Install `Node.js` and `npm`
2. Install `scaleway-cli`: `$ npm install -g scaleway-cli`
3. Setup token and organization: `$ scw login --token=XXXXX --organization=YYYYY`
4. Use `$ scw images`


License
-------

[MIT](https://github.com/moul/scaleway-cli/blob/master/LICENSE.md)
