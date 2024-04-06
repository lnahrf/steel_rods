---
title: "Hacking WiFi 101: basic concepts, terminology, and a real-life example"
seoTitle: "WiFi Hacking Overview: Basics, Terms, Real Example"
seoDescription: "Explore WiFi hacking fundamentals, terms, and practical methods, including security protocols and attacks, for educational use"
datePublished: Wed Apr 03 2024 16:25:58 GMT+0000 (Coordinated Universal Time)
cuid: cluk0s9ms000408jv6ge0chm5
slug: hacking-wifi-101-basic-concepts-terminology-and-a-real-life-example
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/sOdVYQQo4UU/upload/eb85be2bff82671d829b0e3aab807c4f.jpeg
tags: python, hacking, networking, cybersecurity-1

---

This article is written for educational purposes only, *do not ever attempt to hack a network without permission.*

There are multiple ways to get into WiFi networks, it can be as straightforward as clicking a button or as complicated as extremely niche academic research. Some networks are “unhackable” in a realistic time frame, but most of them are between the *"extremely hackable"* and the *"somewhat hackable"* range.

In this article, we will theoretically explain the basic concepts and methods required for you to get an idea of the process of hacking WiFi networks. To do so, we will need to understand some terminology.

### Handshake

When a device is attempting to connect to a WiFi network, it exchanges multiple messages with the access point to establish a secure connection. A "handshake" is the name we give this process of exchanging information between a device and an access point upon connection. This information often contains the cryptographic hash of the network's passcode.

Capturing a handshake allows us to perform different offline attacks on the captured data, without raising alarms or being shut off by the access points' administrators.

### Pcap

"Packet Capture" or .pcap is the file format we use to store the handshake information (or any packet data as a file). Using a .pcap file allows us to perform offline attacks to retrieve the key without ever interacting with the network itself.

### Packet

A network packet is an envelope transferred between two clients in a network. When my router wants to speak with some server, the low-level exchange of data between both entities is sent in packets. A network packet may contain information headers, wrappers, and the transferred data itself.

### Network Adapter

A network adapter or network interface card (NIC) is a physical device required to connect to WiFi networks around you. Some are internal, built directly into your computer's motherboards, or PCI-based. Some are external, and USB-based. For example, the Alfa AWUS036ACH is an external NIC. TP-LINK TG-3468 is an "internal" NIC that connects to your motherboard via PCI.

### Monitor Mode

Monitor mode is a special operation mode for network adapters that allows the adapter to capture all packets sent and received by other adapters while in its range. There is no way to capture a handshake without using a network adapter that supports monitor mode.

### Repeater

A network repeater is a device used to increase the range of a network, essentially transferring data to and from the original router and acting as a middleman between the router and the user over longer distances. *Do not use repeaters, they are the easiest to crack.*

### WPS

“Wifi Protected Setup” is a feature available in a lot of commercial routers used to easily connect devices (such as wireless printers) to your network at the click of a button (without entering a passcode anywhere).

In most cases, WPS is enabled via a physical button on the router. When clicked - the wireless device (printer and so on) can connect to the network without having to authenticate using the traditional method. In some cases, manufacturers ship routers with WPS enabled by default.

### WPS Attack

A WPS attack is a scenario where the attacker is imitating a device (e.g. wireless printer) and requesting to connect to the network using WPS. WPS authentication is protected by 8-digit PIN codes that are fairly easy to crack using brute force methods or are known in public databases and are reused by the router's manufacturer.

Known tools (scripts) that are used to exploit WPS vulnerabilities are [Reaver](https://github.com/t6x/reaver-wps-fork-t6x) and Bully. Another great automated tool is [Airgeddon](https://github.com/v1s1t0r1sh3r3/airgeddon). With some luck, you will be able to run these tools on vulnerable access points (or network repeaters, which are usually vulnerable to WPS attacks) and retrieve the key.

### Wordlist

A "wordlist" is a collection of words or possible passcodes stored in a file. Usually separated by new lines, the wordlist is used as the data for dictionary attacks.

### Bruteforce and Dictionary Attacks

Let's say we have a hash, and we know the key to cracking it is comprised of some combination of numbers and letters. The most straightforward solution would be to try every possible combination of numbers and letters until you get in - *that is brute-forcing your way in.*

A dictionary attack is the same as a bruteforce attack, but this time we will iterate over a pre-defined wordlist and attempt to crack the hash using it instead of values generated during runtime.

From my experience, both of these are the most probable methods of hacking WiFi networks. Once you have captured a handshake, you can write a script, or make a wordlist (which will turn this into a dictionary attack) of the required combinations and attempt to crack it using tools such as [Hashcat](https://hashcat.net/hashcat/).

### Jamming

Jamming is a broader concept relating to the RF field but also applies to the world of WiFi.

WiFi is essentially a radio frequency transmitted by a router on the 2.4Ghz or 5Ghz bands. Jamming is the process of interrupting the transmitted signal, by sending out "noise" and drowning the data transmitted and received by the router, on a specific band.

It is important to note that jamming a signal is a very different concept than de-authenticating.

### De-authentication Attack

De-authentication is an attack where the attacker sends out a "de-authentication" signal (packet) that forces the access point (router) to disconnect from all devices for a brief second. Attackers use de-authentication to force routers to re-connect to nearby devices and thus transmit handshakes, that are then captured by the attackers.

### WEP, WPA and WPA2

WEP, WPA, and WPA2 are security protocols, designed to protect WiFi networks from being hacked. Developed by the WiFi Alliance (yes, it's a thing that exists), WPA was designed to replace the deprecated WEP, and WPA2 added additional security measures to WPA.

### Example

In this example, I will be describing an imaginary situation where an attacker could potentially hack into WiFi networks around my area.

First, we need to capture a handshake. To capture a network's handshake we would usually need to wait patiently for the router to disconnect and reconnect to a device in its range (If we are lucky, this will happen at least once a day). To speed up the process we can send out a signal called a "de-authentication signal". The signal forces the access point to disconnect from all devices for a brief second.

All we have to do is send out a de-authentication packet and attempt to capture a handshake at the same time. This is possible using the right NIC, and tools such as [aircrack-ng](https://www.aircrack-ng.org/doku.php?id=deauthentication).

Once we have captured the handshake as a .pcap file, it's time to crack it. I don't recommend trying to throw random wordlists at it and hoping something will stick. Try and think about the area and the people living in it, how do they act? Are they conscious about security? What will non-techy people set as their WiFi passcode? This will help narrow down options and reduce the hacking process into a realistic timeframe.

In my area, the majority of WiFi passwords are phone numbers in the following format:

```plaintext
0XXXXXXXXX
```

All phone numbers in my area start with 0, the second digit will almost always be 5, and rarely 7. What is left for me to do is to comprise a wordlist of every possible number combination, 8 digits long (the remainder of our template, minus the initial 05).

To achieve this I wrote a Python script:

```python
import numpy as np

def main():
    digits = 8
    prefix = "05" # or 07
    possibilities = 10 ** digits

    template = np.random \
                 .choice(possibilities, size=possibilities, replace=False) \
                 .astype(str)

    lst = list(map(lambda n: n.zfill(digits), template))
    f_lst = [prefix + s for s in lst]
    
    with open('phonenumbers.txt', 'w') as file:
        file.write("\n".join(sorted(f_lst)))
    
    
if __name__ == "__main__": main()
```

I used Python’s numpy library to generate all the required combinations and added the 2-digit static prefix to every line. The script took about 5 minutes to generate a list with a lot of combinations. Now that we have our wordlist we can use tools like Hashcat and our GPU to crack the hash.

### Quicklinks

[Hashcat](https://hashcat.net/hashcat/)

[Cracking WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2)

[Spacehuhn's Deauther](https://github.com/SpacehuhnTech/esp8266_deauther)

[Aircrack-ng](https://www.aircrack-ng.org/)

### Who am I?

My name is Lev, a self-proclaimed hacker and radio enthusiast, writing for my personal tech hub at [HackFM](https://hackfm.com/). I have been tinkering with computers for over a decade, and although I may not always be successful in putting them back together, I have gained some knowledge in the field. I currently work as a senior data engineer and I am always available for consulting or discussing anything tech-related. Want to write for [HackFM](https://hackfm.com/) or collaborate on a gritty technical article? Please let me know!