.. This work is licensed under a Creative Commons Attribution 4.0 International License.
.. http://creativecommons.org/licenses/by/4.0
.. (c) OPNFV, Intel Corporation and others.

Introduction
============

The goal of Software Fastpath service Quality Metrics (SFQM) is to
develop the utilities and libraries in `DPDK`_ to support:

* Measuring Telco Traffic and Performance KPIs. Including:

  * Packet Delay Variation (by enabling TX and RX time stamping).
  * Packet loss (by exposing extended NIC stats).

* Performance Monitoring of the DPDK interfaces (by exposing
  extended NIC stats + collectd Plugin).
* Detecting and reporting violations that can be consumed by VNFs
  and higher level management systems (through DPDK Keep Alive).

After all **the ability to measure and enforce Telco KPIs (Service
assurance) in the data-plane will be mandatory for any Telco grade NFVI
implementation**.

All developed features will be upstreamed to `DPDK`_ or other Open
Source projects relevant to telemetry such as `collectd`_ and
`Ceilometer`_.

The OPNFV project wiki can be found @ `SFQM`_

Problem Statement
==================
Providing carrier grade Service Assurance is critical in the network
transformation to a software defined and virtualized network (NFV).
Medium-/large-scale cloud environments account for between hundreds and
hundreds of thousands of infrastructure systems.  It is vital to monitor
systems for malfunctions that could lead to users application service
disruption and promptly react to these fault events to facilitate improving
overall system performance. As the size of infrastructure and virtual resources
grow, so does the effort of monitoring back-ends. SFQM aims to expose as much
useful information as possible off the platform so that faults and errors in
the NFVI can be detected promptly and reported to the appropriate fault
management entity.

The OPNFV platform (NFVI) requires functionality to:

* Create a low latency, high performance packet processing path (fast path)
  through the NFVI that VNFs can take advantage of;
* Measure Telco Traffic and Performance KPIs through that fast path;
* Detect and report violations that can be consumed by VNFs and higher level
  EMS/OSS systems

Examples of local measurable QoS factors for Traffic Monitoring which impact
both Quality of Experience and five 9's availability would be (using Metro Ethernet
Forum Guidelines as reference):

* Packet loss
* Packet Delay Variation
* Uni-directional frame delay

Other KPIs such as Call drops, Call Setup Success Rate, Call Setup time etc. are
measured by the VNF.

In addition to Traffic Monitoring, the NFVI must also support Performance
Monitoring of the physical interfaces themselves (e.g. NICs), i.e. an ability to
monitor and trace errors on the physical interfaces and report them.

All these traffic statistics for Traffic and Performance Monitoring must be
measured in-service and must be capable of being reported by standard Telco
mechanisms (e.g. SNMP traps), for potential enforcement actions.

Scope
======
The output of the project will provide interfaces and functions to support
monitoring of Packet Latency and Network Interfaces while the VNF is in service.

The DPDK interface/API will be updated to support:

* Exposure of NIC MAC/PHY Level Counters
* Interface for Time stamp on RX
* Interface for Time stamp on TX
* Exposure of DPDK events

collectd will be updated to support the exposure of DPDK metrics and events.

Specific testing and integration will be carried out to cover:

* Unit/Integration Test plans: A sample application provided to demonstrate packet
  latency monitoring and interface monitoring

The following list of features and functionality will be developed:

* DPDK APIs and functions for latency and interface monitoring
* A sample application to demonstrate usage
* collectd plugins

The scope of the project involves developing the relavant DPDK APIs, OVS APIs,
sample applications, as well as the utilities in collectd to export all the
relavent information to a telemetry and events consumer.

VNF specific processing, Traffic Monitoring, Performance Monitoring and
Management Agent are out of scope.

The Proposed Interface counters include:

* Packet RX
* Packet TX
* Packet loss
* Interface errors + other stats

The Proposed Packet Latency Monitor include:

* Cycle accurate stamping on ingress
* Supports latency measurements on egress

Support for failover of DPDK enabled cores is also out of scope of the current
proposal. However, this is an important requirement and must-have functionality
for any DPDK enabled framework in the NFVI. To that end, a second phase of this
project will be to implement DPDK Keep Alive functionality that would address
this and would report to a VNF-level Failover and High Availability mechanism
that would then determine what actions, including failover, may be triggered.

Consumption Models
===================
In reality many VNFs will have an existing performance or traffic monitoring
utility used to monitor VNF behavior and report statistics, counters, etc.

The consumption of performance and traffic related information/events provided
by this project should be a logical extension of any existing VNF monitoring
utility. It should not require a new utility to be developed. We do not see the
Software Fastpath Service Quality Metrics data as major additional effort for
VNFs to consume; this project would be sympathetic to existing VNF architecture
constructs. The intention is that this project represents a lower level
interface for network interface monitoring to be used
by higher level fault management entities (see below).

Allowing the Software Fastpath Service Quality Metrics data to be handled within
existing VNF performance or traffic monitoring utilities also makes it simpler
for overall interfacing with higher level management components in the VIM, MANO
and OSS/BSS. The Software Fastpath Service Quality Metrics proposal would be
complementary to the Fault Management and Maintenance project proposal
(Doctor), which addresses NFVI Fault Management
support in the VIM. To that end, the project committers and contributors for the
Software Fastpath Service Quality Metrics project wish to collaborate with the
Doctor project to facilitate this.

.. _SFQM: https://wiki.opnfv.org/collaborative_development_projects/opnfv_telco_kpi_monitoring
.. _DPDK: http://dpdk.org/
.. _collectd: http://collectd.org/
.. _Ceilometer: https://wiki.openstack.org/wiki/Telemetry
