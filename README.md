# vfs101driver
Automatically exported from code.google.com/p/vfs101driver


Introduction

Fro convenience, this is a recent copy of the readme file in the git repo.
Details

Some tools to examine Validity VFS101 sensors.
Monitoring the device under Windows

To use this, you need:

    Machine with a VFS101 fingerprint reader
    Windows
    UsbSnoop 

Boot up under windows, install UsbSnoop, and monitor the biometric device. Go to Services, stop the DigitalPersona and Validity services, then restart them. Swipe a few fingerprints, then save the UsbSnoop log as UsbSnoop.log.
Analysing the USB dump

To use this, you need:

    Linux
    UsbSnoop.log file from previous step 

Boot up under Linux, copy UsbSnoop.log into this folder, and do this:

    $ cat UsbSnoop.log | ./scripts/UsbSnoop.pl > UsbSnoop.txt
    $ mkdir img2
    $ cat UsbSnoop.txt | ./scripts/Snoop2Api.pl > src/mine.h
    $ gwenview img2 

This should produce:

    a set of API calls in src/mine.h
    PNM images of your fingerprints under img2 

Testing the device under Linux

To use this, you need:

    Machine with a VFS101 fingerprint reader
    Linux 

To import the results of your Windows USB capture, do this:

    $ vi src/proto.c
    ... hook up mine.h, see commit a2fa7c94ee26d233a259fd84538338c7f6b114b1
    ... you will need to rename PREFIX to something else
    $ mkdir bin
    $ make 

Once you've imported your Windows log, or if you just want to use a pre-existing cycle, do this:

    $ mkdir img img/A img/B img/C img/D img/X
    $ ./bin/proto woot personal > output 

This should produce:

    a logfile of Linux vs Windows USB transactions in output
    PNM images of your fingerprints under img 

Personal Information

Personal information is defined as images of your fingerprints, or enough data to produce such images.

The UsbSnoop.log and UsbSnoop.txt files contain enough information to reconstruct images of your fingerprints.

The Snoop2Api.pl script will extract this information and produce PNM images under img2/

By default, ./bin/proto will not produce any personal information. IF you explicitly select a cycle routine (eg: 'woot') AND provide a second argument "personal", then ./bin/proto may (depending on the cycle you select...)

    monitor the fingerprint scanner
    print on stdout enough information to reconstruct your fingerprints
    produce PNM images of your fingerprints under img/ 
