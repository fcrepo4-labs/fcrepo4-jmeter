# fcrepo4-jmeter

## Description

This repository contains JMeter configurations for the Fedora4 Performance and Scalability group.

Test plans can be found [here](https://wiki.duraspace.org/display/FF/Performance+and+Scalability+Test+Plans).

## Usage

To run these Fedora [JMeter](http://jmeter.apache.org/) tests, download JMeter and execute:
```bash
./jmeter -Dfedora_4_server=<default=localhost> -Dfedora_4_context=<default=rest> -n -t <path/to/fcrepo4-jmeter>/fedora.jmx
```
..where`-n` specifies "non-GUI mode", and `-t` specifies the location of the test "jmx" file.

The following is an example running a test against a remote Fedora deployed under the "fcrepo" context:
```bash
./jmeter -Dfedora_4_server=52.90.98.146 -Dfedora_4_context=fcrepo/rest -n -t fedora.jmx
```
