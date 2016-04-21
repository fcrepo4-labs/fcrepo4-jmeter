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

### Running specific performance tests

This JMeter configuration can be used for several of the [test plans](https://wiki.duraspace.org/display/FF/Performance+and+Scalability+Test+Plans)

By default, there are multiple thread groups enabled in `fedora.jmx`.

Enabling or disabling specific thread groups and setting appropriate configuration will run different plans.

#### Test 1 - Size of files - large

The maximum filesize of 10GB as specified in the test plans does not currently seem to work for a couple reasons:
 - the uploaded file is currently a single array, and Java has an array limit of between 1 and 2 billion elements
 - JMeter's HTTP sampler holds the entire request in memory at once
The practical upper limit seems to be around 500MB, which still requires 6-8GB of heap for JMeter.o

Also, generating large random files is CPU-bound, so it's especially preferable
for this test to run jmeter on a separate machine from Fedora. Network
bandwidth is then likely to be the limiting factor - the underlying
RandomStringUtils implementation can generate about 100MB/second of random
data on a i7 3770.

(Ideally instead of generating the entire file at once, we could generate small
chunks at a time and just have the sampler read them from an InputStream, but
that doesn't seem to be possible with the default JMeter HTTP sampler.)

To run with a maximum file size of 500MB:

* Toggle thread groups so only 'Fedora4 Create Binary Resource' is enabled (can only be done via the GUI, but persists after saving)
* Run:
```bash 
JVM_ARGS=-Xmx8G ./jmeter -Dfedora_4_server=<default=localhost> -Dfedora_4_context=<default=rest> -Dfilesize_min=10000 -Dfilesize_max=500000000 -n -t <path/to/fcrepo4-jmeter>/fedora.jmx 
```

#### Test 2 - Size of files - small

* Toggle thread groups so only 'Fedora4 Create Binary Resource' is enabled (can only be done via the GUI, but persists after saving)
* Run:
```bash 
./jmeter -Dfedora_4_server=<default=localhost> -Dfedora_4_context=<default=rest> -Dfilesize_min=0 -Dfilesize_max=4096 -n -t <path/to/fcrepo4-jmeter>/fedora.jmx
```

#### Test 4 - Number of containers - default

* Toggle thread groups so only 'Fedora4 Create New Containers' is enabled (can only be done via the GUI, but persists after saving)
* Run:
```bash
./jmeter -Dfedora_4_server=<default=localhost> -Dfedora_4_context=<default=rest> -n -t <path/to/fcrepo4-jmeter>/fedora.jmx
```
