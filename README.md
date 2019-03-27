### CEG-4900 In class exercise 4
Lets go deeper while using our local ParrotOS virtual machine (or AWS, or any
linux device where you have root...)

### Background
So far the networking tools we have been using (`tcpdump`, `nmap`, python's
`socket()` libraries) all perform a strict set of tasks, but many of the most
basic networking attacks rely on the manipulation of packets.  We have yet to
introduce a tool that truly lets us manipulates packets (easily).  Until now...

[Enter Scapy](https://scapy.net/).

> From [scapy.net](https://scapy.net/):
> Scapy is a powerful interactive packet manipulation program. It is able to forge
> or decode packets of a wide number of protocols, send them on the wire, capture
> them, match requests and replies, and much more. It can easily handle most
> classical tasks like scanning, tracerouting, probing, unit tests, attacks or
> network discovery (it can replace hping, 85% of nmap, arpspoof, arp-sk, arping,
> tcpdump, tethereal, p0f, etc.). It also performs very well at a lot of other
> specific tasks that most other tools can’t handle, like sending invalid frames,
> injecting your own 802.11 frames, combining technics (VLAN hopping+ARP cache
> poisoning, VOIP decoding on WEP encrypted channel, …), etc.


### Resources
* [The scapy python library](https://scapy.net/)
* You ParrotOS linux install from last week


### Task 1 - Installing Scapy
Scapy is pre-installed in ParrotOS so you are already done, if not be sure to
follow last weeks ParrotOS VM installation (class exercise 3).  If you are trying
to use an OS that is not ParrotOS, Scapy is a very common python tool so it should
be easy to find instructions to install.

### Task 2 - Using Scapy interactively
Lets start by performing a basic tcpdump in scapy (and save it to a python
variable)!  Start the scapy ipython interface with the following:
```
sudo scapy
```
This should launch the scapy interface which looks like this:
```
                     aSPY//YASa       
             apyyyyCY//////////YCa       |
            sY//////YSpcs  scpCY//Pp     | Welcome to Scapy
 ayp ayyyyyyySCP//Pp           syY//C    | Version 2.4.2
 AYAsAYYYYYYYY///Ps              cY//S   |
         pCCCCY//p          cSSps y//Y   | https://github.com/secdev/scapy
         SPPPP///a          pP///AC//Y   |
              A//A            cyP////C   | Have fun!
              p///Ac            sC///a   |
              P////YCpc           A//A   | Craft me if you can.
       scccccp///pSP///p          p//Y   |                   -- IPv6 layer
      sY/////////y  caa           S//P   |
       cayCyayP//Ya              pY/Ya
        sY/PsY////YCc          aC//Yp 
         sc  sccaCY//PCypaapyCP//YSs  
                  spCPY//////YPSps    
                       ccaacs         
                                      
>>> 
```
Most of the commands from here on out will be meant to be typed into scapy, *not
Bash*!

* To sniff some packets we are going to use the scapy `sniff()` class
  ```
  capture = sniff(count=10)
  ```

This will sniff 10 packets and store them in capture.  We can print out some
details regarding these packets with the following commands:
* `capture`
* `capture.show()`
* `capture[1]`
* `capture[9].show()`

Play around with the above and look at at least 2 visibly different packets (try
to find a TCP and a UDP or an ICMP packet).

### Task 3 - Lets build a packet!
Scapy provides some awesome tools for packet creation.  Lets look at a few of them:

* `ls()` list available layers OR info on a given layer name (example: `ls(IP)`)
* `lsc()` list available scapy commands
* `IP()` create an IP packet layer
* `TCP()` and `UDP()` both create their respective packet layers
* `show()` and `summary()`

So lets make a packet!
For this example we will be using a DNS queuery for our first packet creation
example.  There are a good number of steps so follow along and feel free to play
around (we aren't sendin these packets anywhere yet, we're just playing).

First lets create an IP packet with just IP().  In scapy:
```
ofp = IP()
ofp.show()
```
Easy right?  `ofp` is just our variable name for Our First Packet (sorry...)

Lets make this IP more useful for our test DNS case and give it a
destination of a nameserver.  If you are on campus you MUST use a campus
nameserver like `130.108.128.200`, otherwise I would just use a google DNS
server such as `8.8.8.8`:
```
ofp = IP(dest="130.108.128.200")
ofp.show()
```

Now we are getting somewhere.  But there isn't much in an IP packet itself
besides source and destination addresses (we can check what needs to go in the
IP section with `ls(IP)`.

Now lets get to some of the good stuff.  In order to get our DNS request to the
server we need to add a UDP layer, which in scapy is done like so:
```
ofudp = ofp/UDP()
```

Here we have created a new packet from the IP layer contained in `ofp` and the
`UDP()` scapy command.  Lets take a look at our two packets now:
```
ofp.show()
ofudp.show()
```

Looking good so far, but we're missing all of the DNS data in the UDP datagram.
Luckily scapy has a library for this too:
```
ofdnsp=ofudp/DNS()
ofdnsp.show()
```
We're almost there, but I wanted to take a second to show you that the DNS()
created an empty shell of the DNS UDP datagram.  To fill that out we need to
enter the necessary information into the scapy `DNS()` field which I do not
expect you to know (however you should be able to capture an example with
tcpdump or scapy and evaluate it).

To make things easier on you I am just providing the correct options below:
```

```


