XRootD Transfer Accounting
==========================

XRootD transfers are accounted by a central collector hosted by the OSG.

![XRootD Transfer Accounting flow](/img/XRootDMonitoringDiagram.png "XRootD Transfer Accounting flow")


## XRootD Servers

XRootD servers are configured to send data to the collector for specific events.  The configuration lines for XRootD (automatically configured in osg-xrootd package):

    xrootd.monitor all \
                   auth \
                   flush 30s \
                   window 5s fstat 60 lfn ops xfr 5 \
                   dest redir fstat info user xrd-report.osgstorage.org:9930 \
                   dest fstat info user xrd-mon.osgstorage.org:9930

The XRootD servers send UDP packets to the two destinations listed in the `xrootd.monitor` configuration.  xrd-mon.osgstorage.org is managed by the OSG, xrd-report.ogsstorage.org is external.

## Collector

The collector is a central server that all XRootD servers send messages.  The collector source is on [GitHub](https://github.com/opensciencegrid/xrootd-monitoring-collector).

The collector receives the UDP packets for accesses to files.    When a file is closed, the summarized data is forwarded to the OSG Message Bus, where it is routed to the WLCG or GRACC depending on the data type.





