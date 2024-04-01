---
title: "Basics and Terminology of Wifi Hacking"
datePublished: Fri Nov 10 2023 09:00:09 GMT+0000 (Coordinated Universal Time)
cuid: close0ezg000d09lah256h1j6
slug: basics-and-terminology-of-wifi-hacking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699748633599/cfd43aa0-b90f-45b4-93ea-5fbe77bd0997.jpeg
tags: python, hacking, wifi, bruteforce, cybersecurity-1

---

This theoretical guide is written for educational purposes only, *do not ever attempt to get access to a network without permission.*

With the disclaimer out of the way, let me get started by saying the first thing I learned how to do as a "script kiddie" was getting access to networks around me. There are multiple ways to get into Wifi networks, it can be as straightforward as clicking a button or as complicated as academic research. Some networks are “unhackable” in a realistic time frame, but most of them are between the *"extremely hackable"* and the *"somewhat hackable"* range.

In this article, we will theoretically explain two relatively simple methods that are efficient (ROI-wise) from my own experience. First, we will need to understand some basic terminology.

### What is a "handshake"?

When a device is attempting to connect to a Wifi network, it exchanges multiple messages with the access point to establish a secure connection. A "handshake" is the name we give this process of exchanging information between a device and an access point upon connection. This information often contains the cryptographic hash of the network's password.

Capturing a "handshake" allows us to perform multiple different offline attacks on the captured data, without raising alarms or being shut off by the access point.

### WPS attack

“Wifi Protected Setup” is a feature available in a lot of commercial routers used to easily connect devices such as wireless printers to your network at the click of a button (without entering a password anywhere).

In most cases, WPS is enabled via a physical button on the router. When clicked - the wireless device (printer and so on) can connect to the network without having to authenticate using traditional methods. In some cases, manufacturers ship routers with WPS enabled by default (without the need to physically click a button on the device).

A WPS attack is a scenario where the attacker is imitating a device (e.g. wireless printer) and requesting to connect to the network using WPS. WPS authentication is protected by 8-digit PIN codes that are fairly easy to crack using brute force methods or are known in public databases and are reused by the router's manufacturer.

Known tools (scripts) that are used to exploit WPS vulnerabilities are "Reaver" and "Bully". With some luck, you will be able to run Reaver or Bully on vulnerable access points (or network repeaters, which are usually vulnerable to WPS attacks) and retrieve the password.

### Bruteforce or Dictionary

Bruteforcing is a concept you have probably encountered over and over in your cybersecurity journey, but the concept remains the same. Imagine you have a hash, and you know the key to cracking it is comprised of numbers and letters. The most straightforward solution would be to try every possible combination of numbers and letters until you get in.

From my experience, this is often the most successful method to hack networks around you. Once you have captured a handshake, you can write a script, or make a wordlist (which will turn this into a dictionary attack) of every possible combination and attempt to crack it using tools such as [Hashcat](https://hashcat.net/hashcat/).

### De-authentication

But first, we need to capture the handshake. To capture a network's handshake we would usually need to wait patiently for the network to disconnect and reconnect to a device in its range (If we are lucky, this will happen at least once a day).

To speed up the process we can send out a signal called a "de-authentication signal". The signal (packet) forces the access point to disconnect from all devices for a brief second. All we have to do is send out a de-authentication packet and attempt to capture a handshake at the same time. This is possible using the right network adapter card, and tools such as [aircrack-ng](https://www.aircrack-ng.org/doku.php?id=deauthentication).

Once we have captured the handshake as a .pcap file, it's time to crack it.

In my area, the majority of Wifi passwords are phone numbers in the following format:

```plaintext
0XXXXXXXXX
```

Most phone numbers in my area start with 0, the second digit will almost always be 5, and rarely 7. What is left for me to do is to comprise a word list of every possible number combination of 8 digits (the remainder of our template, minus the initial 05/07).

To achieve this I have used a simple Python script:

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

I used Python’s NumPy to generate all the required possibilities and added the 2-digit static prefix to every line. The script took about 5 minutes to generate a list with 100,000,000 possibilities. Now that we have a word list (or a phone list) we can use tools like Hashcat and our GPU to crack the hash.

Final note - take a look at Hashcat and their quick guide on [cracking WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2). Using this and the wordlist we created, we should be able to crack a password in less than 10 minutes under the right conditions.