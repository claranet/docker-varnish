
VARNISH - FULL PAGE CACHE
=============================

See project page for more details: https://varnish-cache.org

* __Maintained by__

   [Claranet](https://github.com/claranet/docker-varnish/)

* __Supported architectures__

   amd64

Variables
-----------

Name                 | Description                       | Default
---------------------|-----------------------------------|----------
VARNISH_BACKEND      | Default Backend to use by varnish | localhost
VARNISH_BACKEND_PORT | Backend Port                      | 80
VARNISH_SECRET       | Secret for varnish management     | `pwgen 15`

Default config
----------------

```
vcl 4.0;
import directors;
backend localhost { .host = "${VARNISH_BACKEND}"; .port = "${VARNISH_BACKEND_PORT}"; }
```

Running vanrish
-----------------

Default commandline arguments for Varnish `-F -a :80 -f /etc/varnish/default.vcl -s malloc,1024m -T :6082 -S /etc/varnish/secret`. See https://varnish-cache.org/docs/trunk/reference/varnishd.html#ref-varnishd-options for full list.
