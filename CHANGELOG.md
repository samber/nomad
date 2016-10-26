## 0.5.0 (Unreleased)

IMPROVEMENTS:
  * core: Introduce node SecretID which can be used to minimize the available
    surface area of RPCs to malicious Nomad Clients #1597 
  * agent: Add DataDog telemetry sync #1816
  * api: Support TLS for encrypting Raft, RPC and HTTP APIs #1853
  * cli: `nomad alloc-status` shows allocation creation time #1623
  * cli: `nomad node-status` shows node metadata in verbose mode #1841
  * client: Failed RPCs are retried on all servers #1735
  * client: Enforce shared allocation directory disk usage #1580
  * client: Introduce a `secrets/` directory to tasks where sensitive data can
    be written #1681
  * driver: Export `NOMAD_JOB_NAME` environment variable #1804
  * driver/docker: Support Docker volumes #1767
  * driver/docker: Allow Docker logging to be configured #1767
  * driver/lxc: Support for LXC containers #1699
  * driver/rkt: Support rkt volumes (rkt >= 1.0.0 required) #1812
  * server: Support for gossip encryption #1791

BUG FIXES:
  * agent: Handle the SIGPIPE signal preventing panics on journalctl restarts
    #1802
  * cli: `alloc-status` does not query for allocation statistics if node is down
    #1844
  * client: Prevent race when persisting state file #1682
  * client: Artifact download failures will be retried before failing tasks
    #1558
  * client: Fix a crash related to stats publishing when driver hasn't started
    yet #1723
  * client: Fix a memory leak in the executor that caused failed allocations
    #1762
  * client: Fix old services not getting removed from consul on update #1668
  * client/fingerprint: Fix inconsistent CPU MHz fingerprinting #1366
  * discovery: Fix old services not getting removed from Consul on update
    #1668
  * discovery: Fix HTTP timeout with Server HTTP health check when there is no
    leader #1656
  * discovery: Fix client flapping when server is in a different datacenter as
    the client #1641
  * discovery/jobspec: Validate service name after interpolation #1852
  * driver/docker: Fix `local/` directory mount into container #1830
  * jobspec: Tasks without a resource block no longer fail to validate #1864

## 0.4.1 (August 18, 2016)

__BACKWARDS INCOMPATIBILITIES:__
  * telemetry: Operators will have to explicitly opt-in for Nomad client to
    publish allocation and node metrics

IMPROVEMENTS:
  * core: Allow count 0 on system jobs #1421
  * core: Summarize the current status of registered jobs. [GH-1383, GH-1517]
  * core: Gracefully handle short lived outages by holding RPC calls #1403
  * core: Introduce a lost state for allocations that were on Nodes that died
    #1516
  * api: client Logs endpoint for streaming task logs #1444
  * api/cli: Support for tailing/streaming files [GH-1404, GH-1420]
  * api/server: Support for querying job summaries #1455
  * cli: `nomad logs` command for streaming task logs #1444
  * cli: `nomad status` shows the create time of allocations #1540
  * cli: `nomad plan` exit code indicates if changes will occur #1502
  * cli: status commands support JSON output and go template formating #1503
  * cli: Validate and plan command supports reading from stdin [GH-1460,
    GH-1458]
  * cli: Allow basic authentication through address and environment variable
    #1610
  * cli: `nomad node-status` shows volume name for non-physical volumes instead
    of showing 0B used #1538
  * cli: Support retrieving job files using go-getter in the `run`, `plan` and
    `validate` command #1511
  * client: Add killing event to task state #1457
  * client: Fingerprint network speed on Windows #1443
  * discovery: Support for initial check status #1599
  * discovery: Support for query params in health check urls #1562
  * driver/docker: Allow working directory to be configured #1513
  * driver/docker: Remove docker volumes when removing container #1519
  * driver/docker: Set windows containers network mode to nat by default
    #1521
  * driver/exec: Allow chroot environment to be configurable #1518
  * driver/qemu: Allows users to pass extra args to the qemu driver #1596
  * telemetry: Circonus integration for telemetry metrics #1459
  * telemetry: Allow operators to opt-in for publishing metrics #1501

BUG FIXES:
  * agent: Reload agent configuration on SIGHUP #1566
  * core: Sanitize empty slices/maps in jobs to avoid incorrect create/destroy
    updates #1434
  * core: Fix race in which a Node registers and doesn't receive system jobs
    #1456
  * core: Fix issue in which Nodes with large amount of reserved ports would
    casue dynamic port allocations to fail #1526
  * core: Fix a condition in which old batch allocations could get updated even
    after terminal. In a rare case this could cause a server panic #1471
  * core: Do not update the Job attached to Allocations that have been marked
    terminal #1508
  * agent: Fix advertise address when using IPv6 #1465
  * cli: Fix node-status when using IPv6 advertise address #1465
  * client: Merging telemetry configuration properly #1670
  * client: Task start errors adhere to restart policy mode #1405
  * client: Reregister with servers if node is unregistered #1593
  * client: Killing an allocation doesn't cause allocation stats to block
    #1454
  * driver/docker: Disable swap on docker driver #1480
  * driver/docker: Fix improper gating on priviledged mode #1506
  * driver/docker: Default network type is "nat" on Windows #1521
  * driver/docker: Cleanup created volume when destroying container #1519
  * driver/rkt: Set host environment variables #1581
  * driver/rkt: Validate the command and trust_prefix configs #1493
  * plan: Plan on system jobs discounts nodes that do not meet required
    constraints #1568

## 0.4.0 (June 28, 2016)

__BACKWARDS INCOMPATIBILITIES:__
  * api: Tasks are no longer allowed to have slashes in their name #1210
  * cli: Remove the eval-monitor command. Users should switch to `nomad
    eval-status -monitor`.
  * config: Consul configuration has been moved from client options map to
    consul block under client configuration
  * driver/docker: Enabled SSL by default for pulling images from docker
    registries. #1336

IMPROVEMENTS:
  * core: Scheduler reuses blocked evaluations to avoid unbounded creation of
    evaluations under high contention #1199
  * core: Scheduler stores placement failures in evaluations, no longer
    generating failed allocations for debug information #1188
  * api: Faster JSON response encoding #1182
  * api: Gzip compress HTTP API requests #1203
  * api: Plan api introduced for the Job endpoint #1168
  * api: Job endpoint can enforce Job Modify Index to ensure job is being
    modified from a known state #1243
  * api/client: Add resource usage APIs for retrieving tasks/allocations/host
    resource usage #1189
  * cli: Faster when displaying large amounts ouptuts #1362
  * cli: Deprecate `eval-monitor` and introduce `eval-status` #1206
  * cli: Unify the `fs` family of commands to be a single command #1150
  * cli: Introduce `nomad plan` to dry-run a job through the scheduler and
    determine its effects #1181
  * cli: node-status command displays host resource usage and allocation
    resources #1261
  * cli: Region flag and environment variable introduced to set region
    forwarding. Automatic region forwarding for run and plan #1237
  * client: If Consul is available, automatically bootstrap Nomad Client
    using the `_nomad` service in Consul. Nomad Servers now register
    themselves with Consul to make this possible. #1201
  * drivers: Qemu and Java can be run without an artifact being download. Useful
    if the artifact exists inside a chrooted directory #1262
  * driver/docker: Added a client options to set SELinux labels for container
    bind mounts. #788
  * driver/docker: Enabled SSL by default for pulling images from docker
    registries. #1336
  * server: If Consul is available, automatically bootstrap Nomad Servers
    using the `_nomad` service in Consul.  #1276

BUG FIXES:
  * core: Improve garbage collection of allocations and nodes #1256
  * core: Fix a potential deadlock if establishing leadership fails and is
    retried #1231
  * core: Do not restart successful batch jobs when the node is removed/drained
    #1205
  * core: Fix an issue in which the scheduler could be invoked with insufficient
    state #1339
  * core: Updated User, Meta or Resources in a task cause create/destroy updates
    [GH-1128, GH-1153]
  * core: Fix blocked evaluations being run without properly accounting for
    priority #1183
  * api: Tasks are no longer allowed to have slashes in their name #1210
  * client: Delete tmp files used to communicate with execcutor #1241
  * client: Prevent the client from restoring with incorrect task state #1294
  * discovery: Ensure service and check names are unique [GH-1143, GH-1144]
  * driver/docker: Ensure docker client doesn't time out after a minute.
    #1184
  * driver/java: Fix issue in which Java on darwin attempted to chroot #1262
  * driver/docker: Fix issue in which logs could be spliced #1322

## 0.3.2 (April 22, 2016)

IMPROVEMENTS:
  * core: Garbage collection partitioned to avoid system delays #1012
  * core: Allow count zero task groups to enable blue/green deploys #931
  * core: Validate driver configurations when submitting jobs [GH-1062, GH-1089]
  * core: Job Deregister forces an evaluation for the job even if it doesn't
    exist #981
  * core: Rename successfully finished allocations to "Complete" rather than
    "Dead" for clarity #975
  * cli: `alloc-status` explains restart decisions #984
  * cli: `node-drain -self` drains the local node #1068
  * cli: `node-status -self` queries the local node #1004
  * cli: Destructive commands now require confirmation #983
  * cli: `alloc-status` display is less verbose by default #946
  * cli: `server-members` displays the current leader in each region #935
  * cli: `run` has an `-output` flag to emit a JSON version of the job #990
  * cli: New `inspect` command to display a submitted job's specification
    #952
  * cli: `node-status` display is less verbose by default and shows a node's
    total resources #946
  * client: `artifact` source can be interpreted #1070
  * client: Add IP and Port environment variables #1099
  * client: Nomad fingerprinter to detect client's version #965
  * client: Tasks can interpret Meta set in the task group and job #985
  * client: All tasks in a task group are killed when a task fails #962
  * client: Pass environment variables from host to exec based tasks #970
  * client: Allow task's to be run as particular user [GH-950, GH-978]
  * client: `artifact` block now supports downloading paths relative to the
    task's directory #944
  * docker: Timeout communications with Docker Daemon to avoid deadlocks with
    misbehaving Docker Daemon #1117
  * discovery: Support script based health checks #986
  * discovery: Allowing registration of services which don't expose ports
    #1092
  * driver/docker: Support for `tty` and `interactive` options #1059
  * jobspec: Improved validation of services referencing port labels #1097
  * periodic: Periodic jobs are always evaluated in UTC timezone #1074

BUG FIXES:
  * core: Prevent garbage collection of running batch jobs #989
  * core: Trigger System scheduler when Node drain is disabled #1106
  * core: Fix issue where in-place updated allocation double counted resources
    #957
  * core: Fix drained, batched allocations from being migrated indefinitely
    #1086
  * client: Garbage collect Docker containers on exit #1071
  * client: Fix common exec failures on CentOS and Amazon Linux #1009
  * client: Fix S3 artifact downloading with IAM credentials #1113
  * client: Fix handling of environment variables containing multiple equal
    signs #1115

## 0.3.1 (March 16, 2016)

__BACKWARDS INCOMPATIBILITIES:__
  * Service names that dont conform to RFC-1123 and RFC-2782 will fail
    validation. To fix, change service name to conform to the RFCs before
    running the job #915
  * Jobs that downloaded artifacts will have to be updated to the new syntax and
    be resubmitted. The new syntax consolidates artifacts to the `task` rather
    than being duplicated inside each driver config #921

IMPROVEMENTS:
  * cli: Validate job file schemas #900
  * client: Add environment variables for task name, allocation ID/Name/Index
    [GH-869, GH-896]
  * client: Starting task is retried under the restart policy if the error is
    recoverable #859
  * client: Allow tasks to download artifacts, which can be archives, prior to
    starting #921
  * config: Validate Nomad configuration files #910
  * config: Client config allows reserving resources #910
  * driver/docker: Support for ECR #858
  * driver/docker: Periodic Fingerprinting #893
  * driver/docker: Preventing port reservation for log collection on Unix platforms #897
  * driver/rkt: Pass DNS information to rkt driver #892
  * jobspec: Require RFC-1123 and RFC-2782 valid service names #915

BUG FIXES:
  * core: No longer cancel evaluations that are delayed in the plan queue
    #884
  * api: Guard client/fs/ APIs from being accessed on a non-client node #890
  * client: Allow dashes in variable names during interprelation #857
  * client: Updating kill timeout adheres to operator specified maximum value #878
  * client: Fix a case in which clients would pull but not run allocations
    #906
  * consul: Remove concurrent map access #874
  * driver/exec: Stopping tasks with more than one pid in a cgroup #855
  * executor/linux: Add /run/resolvconf/ to chroot so DNS works #905

## 0.3.0 (February 25, 2016)

__BACKWARDS INCOMPATIBILITIES:__
  * Stdout and Stderr log files of tasks have moved from task/local to
    alloc/logs #851
  * Any users of the runtime environment variable `$NOMAD_PORT_` will need to
    update to the new `${NOMAD_ADDR_}` varriable #704
  * Service names that include periods will fail validation. To fix, remove any
    periods from the service name before running the job #770
  * Task resources are now validated and enforce minimum resources. If a job
    specifies resources below the minimum they will need to be updated #739
  * Node ID is no longer specifiable. For users who have set a custom Node
    ID, the node should be drained before Nomad is updated and the data_dir
    should be deleted before starting for the first time #675
  * Users of custom restart policies should update to the new syntax which adds
    a `mode` field. The `mode` can be either `fail` or `delay`. The default for
    `batch` and `service` jobs is `fail` and `delay` respectively #594
  * All jobs that interpret variables in constraints or driver configurations
    will need to be updated to the new syntax which wraps the interpreted
    variable in curly braces. (`$node.class` becomes `${node.class}`) #760

IMPROVEMENTS:
  * core: Populate job status #663
  * core: Cgroup fingerprinter #712
  * core: Node class constraint #618
  * core: User specifiable kill timeout #624
  * core: Job queueing via blocked evaluations  #726
  * core: Only reschedule failed batch allocations #746
  * core: Add available nodes by DC to AllocMetrics #619
  * core: Improve scheduler retry logic under contention #787
  * core: Computed node class and stack optimization [GH-691, GH-708]
  * core: Improved restart policy with more user configuration #594
  * core: Periodic specification for jobs [GH-540, GH-657, GH-659, GH-668]
  * core: Batch jobs are garbage collected from the Nomad Servers #586
  * core: Free half the CPUs on leader node for use in plan queue and evaluation
    broker #812
  * core: Seed random number generator used to randomize node traversal order
    during scheduling #808
  * core: Performance improvements [GH-823, GH-825, GH-827, GH-830, GH-832,
    GH-833, GH-834, GH-839]
  * core/api: System garbage collection endpoint #828
  * core/api: Allow users to set arbitrary headers via agent config #699
  * core/cli: Prefix based lookups of allocs/nodes/evals/jobs #575
  * core/cli: Print short identifiers and UX cleanup [GH-675, GH-693, GH-692]
  * core/client: Client pulls minimum set of required allocations #731
  * cli: Output of agent-info is sorted #617
  * cli: Eval monitor detects zero wait condition #776
  * cli: Ability to navigate allocation directories [GH-709, GH-798]
  * client: Batch allocation updates to the server #835
  * client: Log rotation for all drivers [GH-685, GH-763, GH-819]
  * client: Only download artifacts from http, https, and S3 #841
  * client: Create a tmp/ directory inside each task directory #757
  * client: Store when an allocation was received by the client #821
  * client: Heartbeating and saving state resilient under high load #811
  * client: Handle updates to tasks Restart Policy and KillTimeout #751
  * client: Killing a driver handle is retried with an exponential backoff
    #809
  * client: Send Node to server when periodic fingerprinters change Node
    attributes/metadata #749
  * client/api: File-system access to allocation directories #669
  * drivers: Validate the "command" field contains a single value #842
  * drivers: Interpret Nomad variables in environment variables/args #653
  * driver/rkt: Add support for CPU/Memory isolation #610
  * driver/rkt: Add support for mounting alloc/task directory #645
  * driver/docker: Support for .dockercfg based auth for private registries
    #773

BUG FIXES:
  * core: Node drain could only be partially applied #750
  * core: Fix panic when eval Ack occurs at delivery limit #790
  * cli: Handle parsing of un-named ports #604
  * cli: Enforce absolute paths for data directories #622
  * client: Cleanup of the allocation directory #755
  * client: Improved stability under high contention #789
  * client: Handle non-200 codes when parsing AWS metadata #614
  * client: Unmounted of shared alloc dir when client is rebooted #755
  * client/consul: Service name changes handled properly #766
  * driver/rkt: handle broader format of rkt version outputs #745
  * driver/qemu: failed to load image and kvm accelerator fixes #656

## 0.2.3 (December 17, 2015)

BUG FIXES:
  * core: Task States not being properly updated #600
  * client: Fixes for user lookup to support CoreOS #591
  * discovery: Using a random prefix for nomad managed services #579
  * discovery: De-Registering Tasks while Nomad sleeps before failed tasks are
    restarted.
  * discovery: Fixes for service registration when multiple allocations are bin
    packed on a node #583
  * configuration: Sort configuration files #588
  * cli: RetryInterval was not being applied properly #601

## 0.2.2 (December 11, 2015)

IMPROVEMENTS:
  * core: Enable `raw_exec` driver in dev mode #558
  * cli: Server join/retry-join command line and config options #527
  * cli: Nomad reports which config files are loaded at start time, or if none
    are loaded #536, [GH-553]

BUG FIXES:
  * core: Send syslog to `LOCAL0` by default as previously documented #547
  * client: remove all calls to default logger #570
  * consul: Nomad is less noisy when Consul is not running #567
  * consul: Nomad only deregisters services that it created #568
  * driver/exec: Shutdown a task now sends the interrupt signal first to the
    process before forcefully killing it. #543
  * driver/docker: Docker driver no longer leaks unix domain socket connections
    #556
  * fingerprint/network: Now correctly detects interfaces on Windows #382

## 0.2.1 (November 28, 2015)

IMPROVEMENTS:

  * core: Can specify a whitelist for activating drivers #467
  * core: Can specify a whitelist for activating fingerprinters #488
  * core/api: Can list all known regions in the cluster #495
  * client/spawn: spawn package tests made portable (work on Windows) #442
  * client/executor: executor package tests made portable (work on Windows) #497
  * client/driver: driver package tests made portable (work on windows) #502
  * client/discovery: Added more consul client api configuration options #503
  * driver/docker: Added TLS client options to the config file #480
  * jobspec: More flexibility in naming Services #509

BUG FIXES:

  * core: Shared reference to DynamicPorts caused port conflicts when scheduling
    count > 1 #494
  * client/restart policy: Not restarting Batch Jobs if the exit code is 0 #491
  * client/service discovery: Make Service IDs unique #479
  * client/service: Fixes update to check definitions and services which are already registered #498
  * driver/docker: Expose the container port instead of the host port #466
  * driver/docker: Support `port_map` for static ports #476
  * driver/docker: Pass 0.2.0-style port environment variables to the docker container #476
  * jobspec: distinct_hosts constraint can be specified as a boolean (previously panicked) #501

## 0.2.0 (November 18, 2015)

__BACKWARDS INCOMPATIBILITIES:__

  * core: HTTP API `/v1/node/<id>/allocations` returns full Allocation and not
    stub #402
  * core: Removed weight and hard/soft fields in constraints #351
  * drivers: Qemu and Java driver configurations have been updated to both use
    `artifact_source` as the source for external images/jars to be ran
  * jobspec: New reserved and dynamic port specification #415
  * jobspec/drivers: Driver configuration supports arbitrary struct to be
    passed in jobspec #415

FEATURES:

  * core: Blocking queries supported in API #366
  * core: System Scheduler that runs tasks on every node #287
  * core: Regexp, version and lexical ordering constraints #271
  * core: distinctHost constraint ensures Task Groups are running on distinct
    clients #321
  * core: Service block definition with Consul registration [GH-463, GH-460,
    GH-458, GH-455, GH-446, GH-425]
  * client: GCE Fingerprinting #215
  * client: Restart policy for task groups enforced by the client [GH-369,
    GH-393]
  * driver/rawexec: Raw Fork/Exec Driver #237
  * driver/rkt: Experimental Rkt Driver [GH-165, GH-247]
  * drivers: Add support for downloading external artifacts to execute for
    Exec, Raw exec drivers #381

IMPROVEMENTS:

  * core: Configurable Node GC threshold #362
  * core: Overlap plan verification and plan application for increased
    throughput #272
  * cli: Output of `alloc-status` also displays task state #424
  * cli: Output of `server-members` is sorted #323
  * cli: Show node attributes in `node-status` #313
  * client/fingerprint: Network fingerprinter detects interface suitable for
    use, rather than defaulting to eth0 [GH-334, GH-356]
  * client: Client Restore State properly reattaches to tasks and recreates
    them as needed [GH-364, GH-380, GH-388, GH-392, GH-394, GH-397, GH-408]
  * client: Periodic Fingerprinting #391
  * client: Precise snapshotting of TaskRunner and AllocRunner [GH-403, GH-411]
  * client: Task State is tracked by client #416
  * client: Test Skip Detection #221
  * driver/docker: Can now specify auth for docker pull #390
  * driver/docker: Can now specify DNS and DNSSearch options #390
  * driver/docker: Can now specify the container's hostname #426
  * driver/docker: Containers now have names based on the task name. #389
  * driver/docker: Mount task local and alloc directory to docker containers #290
  * driver/docker: Now accepts any value for `network_mode` to support userspace networking plugins in docker 1.9
  * driver/java: Pass JVM options in java driver [GH-293, GH-297]
  * drivers: Use BlkioWeight rather than BlkioThrottleReadIopsDevice #222
  * jobspec and drivers: Driver configuration supports arbitrary struct to be passed in jobspec #415

BUG FIXES:

  * core: Nomad Client/Server RPC codec encodes strings properly #420
  * core: Reset Nack timer in response to scheduler operations #325
  * core: Scheduler checks for updates to environment variables #327
  * cli: Fix crash when -config was given a directory or empty path #119
  * client/fingerprint: Use correct local interface on OS X [GH-361, GH-365]
  * client: Nomad Client doesn't restart failed containers #198
  * client: Reap spawn-daemon process, avoiding a zombie process #240
  * client: Resource exhausted errors because of link-speed zero [GH-146,
    GH-205]
  * client: Restarting Nomad Client leads to orphaned containers #159
  * driver/docker: Apply SELinux label for mounting directories in docker
    #377
  * driver/docker: Docker driver exposes ports when creating container [GH-212,
    GH-412]
  * driver/docker: Docker driver uses docker environment variables correctly
    #407
  * driver/qemu: Qemu fingerprint and tests work on both windows/linux #352

## 0.1.2 (October 6, 2015)

IMPROVEMENTS:

  * client: Nomad client cleans allocations on exit when in dev mode #214
  * drivers: Use go-getter for artifact retrieval, add artifact support to
    Exec, Raw Exec drivers #288

## 0.1.1 (October 5, 2015)

IMPROVEMENTS:

  * cli: Nomad Client configurable from command-line #191
  * client/fingerprint: Native IP detection and user specifiable network
    interface for fingerprinting #189
  * driver/docker: Docker networking mode is configurable #184
  * drivers: Set task environment variables #206

BUG FIXES:

  * client/fingerprint: Network fingerprinting failed if default network
    interface did not exist #189
  * client: Fixed issue where network resources throughput would be set to 0
    MBits if the link speed could not be determined #205
  * client: Improved detection of Nomad binary #181
  * driver/docker: Docker dynamic port mapping were not being set properly
    #199

## 0.1.0 (September 28, 2015)

  * Initial release

