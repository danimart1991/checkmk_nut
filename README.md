# checkmk NUT Plugin

[**_Network UPS Tools (NUT)_**](https://networkupstools.org/) _Plugin_ for [**_checkmk 2.0_**](https://checkmk.com/).

Thanks to [@minodudd](https://github.com/minodudd) and [@gurubert](https://github.com/gurubert) for their previous work in this _Plugin_.

## Installation

This _plugin_ has to be installed separately in each of the parts that form a _checkmk_ system.

First, clone or download the repository. Then...

### Agent

We need to install the dependencies:

```bash
$ sudo apt install nut-client
```

Then, copy the file `/usr/lib/check_mk_agent/plugins/nut` to the same path where the agent is installed, and give **0775** permissions:

```bash
$ sudo chmod 775 /usr/lib/check_mk_agent/plugins/nut
```

You could restart the agent with the following commands. If the installation was successful, you should see the _NUT_ info at the end of the output.

```bash
$ systemctl restart check_mk.socket
$ check_mk_agent
...
<<<nut>>>
serverroom_ups battery.charge: 100
serverroom_ups battery.voltage: 13.60
serverroom_ups battery.voltage.high: 13.00
serverroom_ups battery.voltage.low: 10.40
serverroom_ups battery.voltage.nominal: 12.0
serverroom_ups device.type: ups
...
```

### Client (Raw)

Copy the file `/local/lib/check_mk/base/plugins/agent_based/nut.py` to the same path in the _Raw Client_ of _checkmk_ and give **0775** permissions:

```bash
$ sudo chmod 775 /local/lib/check_mk/base/plugins/agent_based/nut.py
```

> If you are using the **_Docker_** _checkmk_ image, surely you need to copy the file to the path `/cmk/local/lib/python3/cmk/base/plugins/agent_based/nut.py` inside the container.

Restart the client/container, and then go to the host where _NUT_ and the _Agent Plugin_ are installed and discover new services with a **_Full Service Scan_**.

## Development

For development, you could modify any of the files that where installed and then restart the _agent_ or the _client_ to test the changes.

Indeed, you could execute this command to discover new services (filtered by _nut_) in the _client_:

```bash
OMD[cmk]:~$ cmk --detect-plugins=nut -vI <HOST>
```

Or, execute this command to obtain the _nut plugin checks_ in the _client_:

```bash
OMD[cmk]:~$ cmk --plugins=nut -v --check nserver
```

> If you are using the **_Docker_** _checkmk_ image, you need to execute the command `omd su cmk` before this commands inside the container.

## References

- https://networkupstools.org/documentation.html
- https://blog.minodudd.com/2013/10/23/ups-monitoring-with-nut-nagios-and-check_mk/
- https://blog.shadypixel.com/monitoring-a-ups-with-nut-on-debian-or-ubuntu-linux/
- https://docs.checkmk.com/latest/en/devel_check_plugins.html
- https://domology.es/integrar-un-sai-en-home-assistant/
