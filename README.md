connectivity-checker
====================

**Table of Contents**

  * [Purpose](#purpose)
  * [Compatibility](#compatibility)
  * [How to build plugin](#how-to-build-plugin)
  * [Known limitations](#known-limitations)
  * [Configuration](#configuration)
  * [How it works](#how-it-works)

## Purpose
The main purpose of this plugin is to provide ability to check network connectivity between nodes closely after network configuration. If there are no full mesh network connectivity between nodes, deployment will be failed. Puppet log of each node will be contain a detailed report which nodes are unaccessible from this node, which networks are affected.

This plugin prevents of wrong network configuration at early deployment stage.

## Compatibility

| Plugin version | Branch        | Fuel version       |
| -------------- | ------------- | ------------------ |
|  9.0.x         | [stable/9.0](https://github.com/xenolog/fuel-plugin-connectivity-checker/tree/stable/9.0)    | Fuel-9.0 (mitaka)  |
|  9.1.x         | [stable/9.1](https://github.com/xenolog/fuel-plugin-connectivity-checker/tree/stable/9.1)    | Fuel-9.1 (mitaka)  |
| 10.0.x         | [stable/10.0](https://github.com/xenolog/fuel-plugin-connectivity-checker/tree/stable/10.0)   | Fuel-10.x (newton) |

__WARNING!__ Do not use plugin from _master_ branch for already released versions of FUEL. Use _stable/xxx_ branch instead, please.  

## How to build plugin

* Install fuel plugin builder (fpb)
* Clone plugin repo and run fpb there:
```
git clone https://github.com/xenolog/fuel-plugin-connectivity-checker
cd fuel-plugin-connectivity-checker
git checkout origin/stable/9.0
fpb --build .
```
* Check if file `connectivity-checker-*.noarch.rpm` was created.

## Known limitations
* Only Task Based Deployment (TBD) method is supported. 

## Configuration
Plugin settings are available on Environment -> Settings -> Other page. Some important notes:
* If `Non-destructive mode` enabled, plugin will not fail deployment process. 
  Only warning will be present into puppet.log if network configuration wrong.
* `Ping tries` -- is a amount of pings, enough for connectivity checking.
* `Ping timeout` -- is a timeout for each ping process for each checking network.

## How it works
General workflow
* This plugin add a `connectivity-checker` task as close as possible after `netconfig` task.
* Connectivity-checker task will wait while ALL `netconfig` tasks on all nodes into the env was done, before run.
* Connectivity-checker task pings each neighbor nodes by each existing network in parallel mode (one ping chain per neighbor node).
* If ping was failed, this result will collected and reported. Deployment will be failed if errors was detected and `non-destructive mode` not enabled.
