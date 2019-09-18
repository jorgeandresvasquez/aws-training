- Throughput:  measures the data transfer rate to/from the storage media in megabytes/second.
- RAID: Redundant Array of Independent Disks
    0:  All disks combined and seen as 1 unit
    1:  Writes everything in both disks
    5:  You need at least 3 disks
        - Last sweep of data is data parity
        - Slower speeds in writing
    6:  Parity is stored on 2 disks, so you can afford to loose up to 2 disks
    10: Raid 1 + 0
        - Excellent read and write speeds
- ITIL:  Information Technology Infrastructure Library, set of detailed practices for IT service management that focuses on aligning IT services with the needs of the business.
- OSI layer:
    - https://www.youtube.com/watch?v=vv4y_uOneC0
- ELK stack:  Elaticsearch, Logstash, Kibana
- Control Plane:  where network signaling, learning, and planning occur
    - ex:  planning public transportation routes
    - In the container world:  responsible for exposing the API and interfaces to define, deploy, and lifecycle containers.
- Data plane:  where the packets are actually moved between endpoints 
    - ex: moving people in buses and trains
    - In the container world:   responsible for providing capacity (as in CPU/Memory/Network/Storage) so that those containers can actually run and connect to a network.
