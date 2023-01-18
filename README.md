# DAB <==> RDK bridge #

This software is a RUST application that enables compatibility with DAB specification to devices based on [Reference Design Kit (RDK)](https://rdkcentral.com/).
The DAB <-> RDK bridge can be executed both on the RDK device or using an external PC.

## Building ##

Since this software uses Cargo package manager, the building process is straightforward:

```
$ cargo build
```

The output binary will be located at `./target/debug/dabridge`.

## Usage ##

```
dabridge --help
dabridge 0.1.0

USAGE:
    dabridge [OPTIONS]

OPTIONS:
    -b, --broker <MQTT_HOST>    The MQTT broker host name or IP (default: localhost)
    -d, --device <DEVICE>       The device host name or IP (default: localhost)
    -h, --help                  Print help information
    -p, --port <MQTT_PORT>      The MQTT broker port (default: 1883)
    -V, --version               Print version information
```

## Examples ##

This bridge supports the three full protocol implementation types, as demonstrated in the [DAB Installation Guide v1.0](https://getdab.org/wp-content/uploads/2021/03/InstallationGuide_v1.0.pdf).

### Option 1: "On Device" Implementation ###

![Option 1: "On Device" Implementation](doc/Option1.png)

```
$ dabridge
```

### Option 2: Remote Broker Implementation ###

![Option 2: Remote Broker Implementation](doc/Option2.png)

Let's suppose `192.168.0.100` as the MQTT Broker IP address:

```
$ dabridge -b 192.168.0.100
```

### Option 3: "Bridge" Implementation ###

![Option 3: "Bridge" Implementation](doc/Option3.png)

Let's suppose `192.168.0.200` as the RDK Device (Device Under Test) IP address:

```
$ dabridge -d 192.168.0.200
```

## DAB Operations Currently Supported ##

This version implements support for the Device Automation Bus (DAB) Protocol Specification document version `Version: 1.0 | Last Updated: 2021-03-01`. It currently supports the following DAB operations:

### Applications ###

| Request Topic                        | Supported |
|--------------------------------------|-----------|
| dab/applications/list                | Yes       |
| dab/applications/launch              | Yes       |
| dab/applications/launch-with-content | -         |
| dab/applications/get-state           | Yes       |
| dab/applications/exit                | Yes       |
| dab/device/info                      | Yes       |
| dab/system/restart                   | Yes       |
| dab/system/language/list             | -   [1]   |
| dab/system/language/get              | - [1]   |
| dab/system/language/set              | - [1]   |

*[1] Currently Accelerator UI is not integrated with RDK's language settings file.*

### Input ###

| Request Topic                        | Supported |
|--------------------------------------|-----------|
| dab/input/key-press                  | Yes       |
| dab/input/long-key-press             | -         |

#### Key map ####

|	Key	                  |	Code	|
|-----------------------|-------|
|	KEY_POWER	            |	112  	|
|	KEY_VOLUME_UP	        |	175	  |
|	KEY_VOLUME_DOWN	      |	174	  |
|	KEY_MUTE	            |	173	  |
|	KEY_CHANNEL_UP	      |	175	  |
|	KEY_CHANNEL_DOWN	    |	174	  |
|	KEY_MENU	            |	-	    |
|	KEY_EXIT            	|	36  	|
|	KEY_INFO            	|	-	    |
|	KEY_GUIDE	            |	-	    |
|	KEY_UP	              |	38	  |
|	KEY_PAGE_UP	          |	-	    |
|	KEY_PAGE_DOWN         |	-	    |
|	KEY_RIGHT	            |	39	  |
|	KEY_DOWN	            |	40	  |
|	KEY_LEFT	            |	37	  |
|	KEY_ENTER	            |	13	  |
|	KEY_BACK	            |	-	    |
|	KEY_PLAY	            |	-	    |
|	KEY_PLAY_PAUSE	      |	-	    |
|	KEY_PAUSE	            |	-	    |
|	KEY_RECORD	          |	-	    |
|	KEY_STOP	            |	-	    |
|	KEY_REWIND	          |	-	    |
|	KEY_FAST_FORWARD     	|	-	    |
|	KEY_SKIP_REWIND	      |	-	    |
|	KEY_SKIP_FAST_FORWARD	|	-	    |
|	KEY_0	                |	48  	|
|	KEY_1	                |	49  	|
|	KEY_2	                |	50  	|
|	KEY_3	                |	51  	|
|	KEY_4	                |	52  	|
|	KEY_5	                |	53  	|
|	KEY_6	                |	54  	|
|	KEY_7	                |	55  	|
|	KEY_8	                |	56  	|
|	KEY_9	                |	57  	|
|	KEY_RED	              |	-	    |
|	KEY_GREEN	            |	-	    |
|	KEY_YELLOW	          |	-	    |
|	KEY_BLUE              |	-	    |


### Device and Application Telemetry ###

| Request Topic                        | Supported |
|--------------------------------------|-----------|
| dab/device-telemetry/start           | -         |
| dab/device-telemetry/stop            | -         |
| dab/app-telemetry/start              | -         |
| dab/app-telemetry/stop               | -         |
| dab/device-telemetry/metrics         | -         |
| dab/app-telemetry/metrics/<appId>    | -         |

### Health Check ###

| Request Topic                        | Supported |
|--------------------------------------|-----------|
| dab/health-check/get                 | Yes       |

### General Notifications ###

| Notification Topic                   | Supported |
|--------------------------------------|-----------|
| dab/messages                         | Yes       |

### Versioning of the protocol ###

| Notification Topic                   | Supported |
|--------------------------------------|-----------|
| dab/version                          | Yes       |