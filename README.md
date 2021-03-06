# Arc Lamp

Arc Lamp comprises a set of scripts for getting useful performance data
out of HHVM's Xenon extension. The Xenon extension captures a trace of
PHP code at regular intervals. When aggregated, the data generated by
the Xenon extension call help you determine where your program is
spending the most time.

## Prerequisites

* A PHP application running on HHVM.
* A Redis server.
* flamegraph.pl from [brendangregg/FlameGraph](https://github.com/brendangregg/FlameGraph)

## Quick start

First, enable the Xenon extension in HHVM's INI file by setting
`hhvm.xenon.period`.  This setting specifies the interval (in seconds)
between trace captures on each host.  If your PHP application is served
by just one server, you can set this value to 60 to make Xenon capture a
trace once a minute. If you have more than one server, you can use the
following formula to help you choose a value:

```ini
hhvm.xenon.period =  60 * servers / desired_number_of_samples_per_minute
```

So, for example, if you want to log a trace every 30 seconds, and you
have 10 PHP application servers, the value is 60 * 10 / 0.5 = 300.

```php
require_once 'ArcLamp.php';

register_shutdown_function( 'ArcLamp\logXenonData', array( '127.0.0.1', 6379 ) );
```

## License

Copyright 2014 Ori Livneh <ori@wikimedia.org>

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License.  You may obtain
a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
