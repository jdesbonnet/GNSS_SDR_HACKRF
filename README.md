# GNSS_SDR_HACKRF

Experiments in getting GNSS-SDR working with the HackRF One SDR. 

Summary: while all the hardware seems to be working, there is no evidence of the reception of any kind of GPS L1 signal. The GNSS antenna is known to work in regular positioning and RTK modes. The config file is from the documentation (the HackRF example). So that leaves the HackRF which *seems* to work (eg gqrx shows signals where I expect them, Dump1090 works, kalibrate reports GSM channels). I have also verified that there is a 3.22V DC bias on the antenna port when running gnss-sdr.  While running, file 'observables.dat' grows in the working directory, but it compresses to 0.1% with gzip: all zeros. So it's not getting any ADC data. 

What to check next??
 * Check that M8P uses 3.3V bias (almost certain it does)... maybe my antennas DC bias needs to be higher. 
 * Capture signal using hackrf_transfer tool, compare with examples and attempt to post-process.

## Equipment:

 * HackRF One (Great Scott Gadgets)
 * 3 band active GNSS antenna with SMA connector (from NavSpark shop). This antenna has been confirmed to work with regular and RTK positioining.
 * Dell T1650 quad-core Xeon, 16GB RAM running Ubuntu 18.04 64 bit.
 
## Software:
 
 * gnss-sdr (version 0.0.9) from default Ubuntu repositories
 * gnss-sdr (built from source in 'next' branch, last commit 20 Nov 2018)

## Configuration:

See conf files in repository.


## Results to date

Command:

```
../gnss-sdr/build/src/main/gnss-sdr --config_file=GPS_L1_hackrf.next_branch.conf
```

Output:

```
linux; GNU C++ version 7.3.0; Boost_106501; UHD_003.010.003.000-0-unknown

Initializing GNSS-SDR v0.0.9.git-next-4810bdd0 ... Please wait.
Logging will be written at "/tmp"
Use gnss-sdr --log_dir=/path/to/log to change that.
OsmoSdr arguments: hackrf,bias=1
gr-osmosdr 0.1.4 (0.1.4) gnuradio 3.7.11
built-in source types: file osmosdr fcd rtl rtl_tcp uhd miri hackrf bladerf rfspace airspy airspyhf soapy redpitaya freesrp 
Using HackRF One with firmware 2014.08.1 
Enabled antenna bias voltage
Actual RX Rate: 2000000.000000 [SPS]...
Actual RX Freq: 1575420000.000000 [Hz]...
PLL Frequency tune error 0.000000 [Hz]...
Actual RX Gain: 14.000000 dB...
Actual Bandwidth: 6000000.000000 [Hz]...
WARNING: Implementation 'GPS_L1_CA_Observables' of the Observables block has been replaced by 'Hybrid_Observables'.
Please update your configuration file.
Starting a TCP/IP server of RTCM messages on port 2101
The TCP/IP server of RTCM messages is up and running. Accepting connections ...
TcpCmdInterface: Telecommand TCP interface listening on port 3333
Current receiver time: 1 s
Tracking of GPS L1 C/A signal started on channel 0 for satellite GPS PRN 01 (Block IIF)
Tracking of GPS L1 C/A signal started on channel 1 for satellite GPS PRN 09 (Block IIF)
Tracking of GPS L1 C/A signal started on channel 2 for satellite GPS PRN 10 (Block IIF)
Tracking of GPS L1 C/A signal started on channel 3 for satellite GPS PRN 11 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 4 for satellite GPS PRN 12 (Block IIR-M)
Tracking of GPS L1 C/A signal started on channel 5 for satellite GPS PRN 13 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 6 for satellite GPS PRN 14 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 7 for satellite GPS PRN 15 (Block IIR-M)
Current receiver time: 2 s
Loss of lock in channel 0!
Tracking of GPS L1 C/A signal started on channel 0 for satellite GPS PRN 01 (Block IIF)
Loss of lock in channel 1!
Tracking of GPS L1 C/A signal started on channel 1 for satellite GPS PRN 09 (Block IIF)
Loss of lock in channel 2!
Loss of lock in channel 3!
Tracking of GPS L1 C/A signal started on channel 2 for satellite GPS PRN 10 (Block IIF)
Loss of lock in channel 4!
Tracking of GPS L1 C/A signal started on channel 3 for satellite GPS PRN 16 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 4 for satellite GPS PRN 17 (Block IIR-M)
Loss of lock in channel 5!
Loss of lock in channel 6!
Tracking of GPS L1 C/A signal started on channel 5 for satellite GPS PRN 13 (Block IIR)
Loss of lock in channel 7!
Tracking of GPS L1 C/A signal started on channel 6 for satellite GPS PRN 18 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 7 for satellite GPS PRN 19 (Block IIR)
Current receiver time: 3 s
Loss of lock in channel 0!
Tracking of GPS L1 C/A signal started on channel 0 for satellite GPS PRN 01 (Block IIF)
Loss of lock in channel 1!
Tracking of GPS L1 C/A signal started on channel 1 for satellite GPS PRN 09 (Block IIF)
Loss of lock in channel 2!
Loss of lock in channel 3!
Tracking of GPS L1 C/A signal started on channel 2 for satellite GPS PRN 10 (Block IIF)
Loss of lock in channel 4!
Tracking of GPS L1 C/A signal started on channel 3 for satellite GPS PRN 20 (Block IIR)
Tracking of GPS L1 C/A signal started on channel 4 for satellite GPS PRN 21 (Block IIR)
Loss of lock in channel 5!
```

These Tracking / Loss of lock messages keep repeating, with no other type of message ever after 20 minutes+ of running.

With the version from Ubuntu repositories (v 0.0.9):

```
linux; GNU C++ version 7.3.0; Boost_106501; UHD_003.010.003.000-0-unknown

Initializing GNSS-SDR v0.0.9 ... Please wait.
Logging will be done at "/tmp"
Use gnss-sdr --log_dir=/path/to/log to change that.
OsmoSdr arguments: hackrf,bias=1
gr-osmosdr 0.1.4 (0.1.4) gnuradio 3.7.11
built-in source types: file osmosdr fcd rtl rtl_tcp uhd miri hackrf bladerf rfspace airspy airspyhf soapy redpitaya freesrp 
Using HackRF One with firmware 2014.08.1 
Enabled antenna bias voltage
Actual RX Rate: 2000000.000000 [SPS]...
Actual RX Freq: 1575420000.000000 [Hz]...
PLL Frequency tune error 0.000000 [Hz]...Actual RX Gain: 14.000000 dB...
Current input signal time = 1 [s]
Current input signal time = 2 [s]
Current input signal time = 3 [s]
Current input signal time = 4 [s]
Current input signal time = 5 [s]
Current input signal time = 6 [s]
Current input signal time = 7 [s]
Current input signal time = 8 [s]
Current input signal time = 9 [s]
```

(this goes on... no other type of message is displayed, even after 20 minutes +)


# Testing HackRF 

hackrf_info

```
hackrf_info version: unknown
libhackrf version: unknown (0.5)
Found HackRF
Index: 0
Board ID Number: 2 (HackRF One)
Firmware Version: 2014.08.1 (API:1.00)
Part ID Number: 0xa000cb3c 0x004f4759
```

Using kalibrate-hackrf:

./src/kal -s GSM900

```
joe@joe-Precision-T1650:/var/tmp/gnss-sdr/kalibrate-hackrf$ ./src/kal  -s GSM900
kal: Scanning for GSM-900 base stations.
GSM-900:
	chan:   52 (945.4MHz + 39.593kHz)	power: 1001135.09
	chan:   53 (945.6MHz + 14.702kHz)	power: 1072053.73
	chan:   54 (945.8MHz - 2.277kHz)	power: 1008929.00
	chan:   55 (946.0MHz - 35.274kHz)	power: 1045148.52
	chan:  104 (955.8MHz - 2.281kHz)	power:  689166.83
^C
joe@joe-Precision-T1650:/var/tmp/gnss-sdr/kalibrate-hackrf$ ./src/kal  -s GSM900
kal: Scanning for GSM-900 base stations.
GSM-900:
	chan:   52 (945.4MHz + 39.590kHz)	power: 2140210.84
	chan:   53 (945.6MHz + 39.715kHz)	power: 2291843.19
	chan:   54 (945.8MHz - 10.295kHz)	power: 2407921.34
	chan:   55 (946.0MHz - 35.301kHz)	power: 2528721.18
	chan:  104 (955.8MHz + 14.691kHz)	power: 1362716.30
	chan:  119 (958.8MHz + 6.901kHz)	power: 1329559.36
	chan:  120 (959.0MHz - 24.528kHz)	power: 1484227.81

```

HackRF device also confirmed to work with Dump1090 using simple telescopic antenna.

```

*a8000a80fb133f06c00421e05c20;
CRC: e05c20 (ok)
DF 21: Comm-B, Identity Reply.
  Flight Status  : Normal, Airborne
  DR             : 0
  UM             : 0
  Squawk         : 7000
  ICAO Address   : 4ca70d

*a8000a8000000030a40000fb7e68;
CRC: fb7e68 (ok)
DF 21: Comm-B, Identity Reply.
  Flight Status  : Normal, Airborne
  DR             : 0
  UM             : 0
  Squawk         : 7000
  ICAO Address   : 4ca70d

```
