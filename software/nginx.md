# Nginx Installtion from source


## Debian 7.6
```sh
apt-get update
apt-get upgrade
apt-get install curl gcc make g++ openssl libssl-dev libpcre3-dev

# zlib Installation
curl -O http://zlib.net/zlib-1.2.8.tar.gz
tar -xzf zlib-1.2.8.tar
cd zlib-1.2.8
./configure
make
make install
cd ..
rm -rf zlib-1.2.8

# Nginx Installation
curl -O http://nginx.org/download/nginx-1.6.0.tar.gz 
tar -xzf nginx-1.6.0.tar.gz 


./configure \
--user=nginx                          \
--group=nginx                         \
--prefix=/etc/nginx                   \
--sbin-path=/usr/sbin/nginx           \
--conf-path=/etc/nginx/nginx.conf     \
--pid-path=/var/run/nginx.pid         \
--lock-path=/var/run/nginx.lock       \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module        \
--with-http_stub_status_module        \
--with-http_ssl_module                \
--with-pcre                           \
--with-file-aio                       \
--with-http_realip_module             \
--without-http_scgi_module            \
--without-http_uwsgi_module           \
--without-http_fastcgi_module

nano /etc/init.d/nginx

#!/bin/sh

### BEGIN INIT INFO
# Provides:           nginx
# Required-Start:     $local_fs $remote_fs $network $syslog $named
# Required-Stop:      $local_fs $remote_fs $network $syslog $named
# Default-Start:      2 3 4 5
# Default-Stop:       0 1 6
# Short-Description:  nginx LSB init script
# Description:        nginx Linux Standards Base compliant init script.
### END INIT INFO

# -----------------------------------------------------------------------------
# This is free and unencumbered software released into the public domain.
#
# Anyone is free to copy, modify, publish, use, compile, sell, or
# distribute this software, either in source code form or as a compiled
# binary, for any purpose, commercial or non-commercial, and by any
# means.
#
# In jurisdictions that recognize copyright laws, the author or authors
# of this software dedicate any and all copyright interest in the
# software to the public domain. We make this dedication for the benefit
# of the public at large and to the detriment of our heirs and
# successors. We intend this dedication to be an overt act of
# relinquishment in perpetuity of all present and future rights to this
# software under copyright law.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
# OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
# ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
# OTHER DEALINGS IN THE SOFTWARE.
#
# For more information, please refer to <http://unlicense.org>
# -----------------------------------------------------------------------------

# -----------------------------------------------------------------------------
# nginx Linux Standards Base compliant init script.
#
# LINK:       https://wiki.debian.org/LSBInitScripts
# IDEAS BY:   Karl Blessing <http://kbeezie.com/debian-ubuntu-nginx-init-script/>
# AUTHOR:     Richard Fussenegger <richard@fussenegger.info>
# COPYRIGHT:  Copyright (c) 2013 Richard Fussenegger
# LICENSE:    http://unlicense.org/ PD
# LINK:       http://richard.fussenegger.info/
# -----------------------------------------------------------------------------


# -----------------------------------------------------------------------------
#                                                                      Includes
# -----------------------------------------------------------------------------


# Load the LSB log_* functions.
. /lib/lsb/init-functions


# -----------------------------------------------------------------------------
#                                                                     Variables
# -----------------------------------------------------------------------------


# The name of the service (must be the first variable).
NAME="nginx"

# Absolute path to the executable.
DAEMON="/usr/sbin/${NAME}"

# Arguments that should be passed to the executable.
DAEMON_ARGS=""

# Absolute path to the PID file.
PIDFILE="/var/run/${NAME}.pid"


# -----------------------------------------------------------------------------
#                                                                     Bootstrap
# -----------------------------------------------------------------------------


# Check return status of EVERY command
set -e

# Check if ${NAME} is a file and executable, if not assume it's not installed.
if [ ! -x ${DAEMON} ]; then
  log_failure_msg ${NAME} "not installed"
  exit 1
fi

# This script is only accessible for root (sudo).
if [ $(id -u) != 0 ]; then
  log_failure_msg "super user only!"
  exit 1
fi

# Always check if service is already running.
RUNNING=$(start-stop-daemon --start --quiet --pidfile ${PIDFILE} --exec ${DAEMON} --test && echo "false" || echo "true")


# -----------------------------------------------------------------------------
#                                                                     Functions
# -----------------------------------------------------------------------------


###
# Reloads the service.
#
# RETURN:
#   0 - successfully reloaded
#   1 - reloading failed
###
reload_service() {
  start-stop-daemon --stop --signal HUP --quiet --pidfile ${PIDFILE} --exec ${DAEMON}
}

###
# Starts the service.
#
# RETURN:
#   0 - successfully started
#   1 - starting failed
###
start_service() {
  start-stop-daemon --start --quiet --pidfile ${PIDFILE} --exec ${DAEMON} -- ${DAEMON_ARGS}
}

###
# Stops the service.
#
# RETURN:
#   0 - successfully stopped
#   1 - stopping failed
###
stop_service() {
  start-stop-daemon --stop --quiet --pidfile ${PIDFILE} --name ${NAME}
}


# -----------------------------------------------------------------------------
#                                                                  Handle Input
# -----------------------------------------------------------------------------


case ${1} in

  force-reload|reload)
    if [ ${RUNNING} = "false" ]; then
      log_failure_msg ${NAME} "not running"
    else
      log_daemon_msg ${NAME} "reloading configuration"
      reload_service || log_end_msg 1
      log_end_msg 0
    fi
  ;;

  restart)
    if [ ${RUNNING} = "false" ]; then
      log_failure_msg ${NAME} "not running"
    else
      log_daemon_msg ${NAME} "restarting"
      stop_service || log_end_msg 1
      sleep 0.1
      start_service || log_end_msg 1
      log_end_msg 0
    fi
  ;;

  start)
    if [ ${RUNNING} = "true" ]; then
      log_success_msg ${NAME} "already started"
    else
      log_daemon_msg ${NAME} "starting"
      start_service || log_end_msg 1
      log_end_msg 0
    fi
  ;;

  status)
    status_of_proc ${DAEMON} ${NAME} && exit 0 || exit ${?}
  ;;

  stop)
    if [ ${RUNNING} = "false" ]; then
      log_success_msg ${NAME} "already stopped"
    else
      log_daemon_msg ${NAME} "stopping"
      stop_service && log_end_msg 0 || log_end_msg 1
    fi
  ;;

  *)
    echo "Usage: ${NAME} {force-reload|reload|restart|start|status|stop}" >&2
    exit 1
  ;;

esac
:

exit 0

chmod +x /etc/init.d/nginx



```

## CentOS 7 (Not Working)
```sh
yum update
yum -y install gcc gcc-c++ make zlib-devel pcre-devel openssl-devel
curl -O http://nginx.org/download/nginx-1.6.0.tar.gz 
tar -xzf nginx-1.6.0.tar.gz 


./configure \
--user=nginx                          \
--group=nginx                         \
--prefix=/etc/nginx                   \
--sbin-path=/usr/sbin/nginx           \
--conf-path=/etc/nginx/nginx.conf     \
--pid-path=/var/run/nginx.pid         \
--lock-path=/var/run/nginx.lock       \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module        \
--with-http_stub_status_module        \
--with-http_ssl_module                \
--with-pcre                           \
--with-file-aio                       \
--with-http_realip_module             \
--without-http_scgi_module            \
--without-http_uwsgi_module           \
--without-http_fastcgi_module

make
make install

useradd -r nginx
nano /etc/init.d/nginx
chmod +x /etc/init.d/nginx
chkconfig --add nginx
chkconfig --level 345 nginx on




```
