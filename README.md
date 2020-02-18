# httpd-openidc-debug
Docker container to debug apache (httpd) with mod_auth_openidc

## How to provoke a segfault through reload (graceful restart)
1. Run container
    ```bash
    docker run --name httpd-openidc-debug -d ifrido/httpd-openidc-debug:latest
    ```
1. Execute shell in running container
    ```bash
    docker exec -ti httpd-openidc-debug /bin/bash
    ```
1. Inside container
    1. Start httpd
        ```bash
        /opt/apache-2.4.41/bin/apachectl start
        ```
    1. Reload httpd
        ```bash
        /opt/apache-2.4.41/bin/apachectl graceful
        ```
    1. Check for segfault in error log
        ```bash
        tail -1 /opt/apache-2.4.41/logs/error_log
        [Tue Feb 18 21:01:24.244620 2020] [core:notice] [pid 22:tid 140590183574400] AH00051: child pid 23 exit signal Segmentation fault (11), possible coredump in /tmp/coredump
        ```
    1. Examine coredump with gdb
        ```bash
        gdb /opt/apache-2.4.41/bin/httpd /tmp/coredump/core
        ```
