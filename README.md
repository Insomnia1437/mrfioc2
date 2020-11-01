# mrfioc2-evtlog Version

*Save all the timing system event codes on disk for further diagnosis (either ascii format or binary)*


## Features:

- Based on mrfioc2-2-2.0
- Modified file `evrMrmApp\src\drvem.cpp` and `evrMrmApp\src\drvem.cpp` only
- Only tested VME-EVR-230RF on VxWorks
- EPICS BASE should >= 3.15.1 (utilization of thread safe versions of ring buffers)
- Need NFS (why not Channel Access? [check this paper for the detail](https://www.pasj.jp/web_publish/pasj2019/proceedings/PDF/FRPH/FRPH001.pdf))
- Log files hierarchy is like `/YEAR/MONTH/DAY/HOUR.log`
- Log data format is like `code-second.nanosecond`
- Timestamp reset event `125` is also logged
- Support Several EVRs (theoretically)

## How to use

Apart from normal mrfioc2 module, you need to configure NFS Mount and some parameters in `st.cmd`.

i.e.

```shell
hostAdd "linacdisk11", "172.19.64.150"

nfsAuthUnixSet("linacdisk11",12681,12600,0,0)

nfsMount "linacdisk11","/vol01/users/sdcswd/epics/R3.15.5/ioc/evtlog/evtlog", "/evtlog"

drvemEvtLogConfigure("/evtlog", 50000, 15, 1)
```

the parameters of `drvemEvtLogConfigure` iocsh function:

1. directory used to save event log files
2. ring buffer size, default is 50000
3. evtlog thread sleep time during the iocInit, default is 15, set 0 to disable event logging.
4. format of log file
    - `1`: ascii file
    - `2`: binary file

------------------


What is Available?
------------------

More infomation on the Micro Research hardware can be found on their
website http://www.mrf.fi/.

Documentation appears at [http://epics.sourceforge.net/mrfioc2](http://epics.sourceforge.net/mrfioc2)

The latest developments can be found in the 'mrfioc2' VCS repository.

[https://github.com/epics-modules/mrfioc2](https://github.com/epics-modules/mrfioc2)

Prerequisites
-------------

The required software is EPICS Base >= 3.14.10, devLib2 >= 2.8, and the MSI tool (included in Base >= 3.15.1).

Base: [http://www.aps.anl.gov/epics/base/R3-15/index.php](http://www.aps.anl.gov/epics/base/R3-15/index.php)

devLib2: [https://github.com/epics-modules/devlib2/](https://github.com/epics-modules/devlib2/)

MSI (Base < 3.15 only): [http://www.aps.anl.gov/epics/extensions/msi/index.php](http://www.aps.anl.gov/epics/extensions/msi/index.php)

The Source
----------

VCS Checkout

```shell
git clone https://github.com/epics-modules/mrfioc2.git
```

Edit 'configure/CONFIG_SITE' and run "make".

<a href="https://travis-ci.org/epics-modules/mrfioc2"><img src="https://travis-ci.org/epics-modules/mrfioc2.svg">CI Build Status</img></a>
