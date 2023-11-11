---
title: "Internet Cafe: Basics of getting access into Wifi networks around you"
datePublished: Fri Nov 10 2023 09:00:09 GMT+0000 (Coordinated Universal Time)
cuid: close0ezg000d09lah256h1j6
slug: internet-cafe-basics-of-getting-access-into-wifi-networks-around-you
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/2m71l9fA6mg/upload/bee6c8ae441cf6a08b532791c7cd5127.jpeg
tags: python, hacking, wifi, bruteforce, cybersecurity-1

---

This article is titled "Internet Cafe" because it is important to remember that you will be better off using the nearest internet cafe, instead of attempting to force your way into networks around you. This theoretical guide is written for educational purposes only, *do not ever attempt to get access to a network that you do not own.*

With the disclaimer out of the way, let me get started by saying the first thing I learned how to do when I dabbled in hacking and pen-testing was getting access to networks around me. There are multiple ways to get into Wifi networks, it can be as straightforward as clicking a button or as complicated as academic research about the “logic behind randomicity in nature”. Some networks are “unhackable” in a realistic time frame, but most of them are between the *"extremely hackable"* and the *"somewhat hackable"* range.

Today we will theoretically explain two relatively simple methods that are efficient (ROI-wise) from my own experience. First, we will need to understand some basic terminology.

### What is a Wifi "handshake"?

When a device is connecting to a Wifi network, it exchanges multiple messages with the access point to establish a secure connection. A "handshake" is the name we give this process of exchanging information between a device and an access point upon connection. This information often contains the cryptographic hash of the network's password.

Capturing a "handshake" allows us to perform multiple different offline attacks (on the captured data) without raising alarms or being shut off by the access point.

### WPS attack

WPS stands for “Wifi Protected Setup”, it is a feature available in a lot of commercial routers used to connect devices such as wireless printers to the network at the click of a button (without entering a password).

Most of the time, WPS is enabled through a physical button on the router, when clicked - the wireless device (printer and so on) can connect to the network without having to authenticate using traditional methods. In some cases, manufacturers ship routers with WPS enabled by default (without the need to physically click a button on the device).

A WPS attack is a scenario where the attacker is imitating a device (such as a wireless printer) and requesting to connect to the network using WPS. WPS authentication is protected by 8-digit PIN codes that are fairly easy to crack using brute force methods or are previously known in public databases and are reused by the router's manufacturer.

Known tools (scripts) that are used to exploit WPS vulnerabilities are Reaver and Bully. With some luck, you will be able to run Reaver or Bully on vulnerable access points (or network repeaters, which are usually vulnerable to WPS attacks) and get the password.

### Bruteforce or Dictionary

Bruteforcing is a concept you have probably encountered over and over in your cybersecurity journey, the same principles apply. If you have a hash, and you know the key is comprised of numbers and letters, the most straightforward solution would be to try every possible combination until you get in.

From my experience, this is often the most successful method of getting into Wifi networks around you. Once you have captured a handshake, you can write a script, or make a wordlist (which will turn it into a dictionary attack) of every possible combination and go to town using tools like [Hashcat](https://hashcat.net/hashcat/).

### De-authentication

But first, we need to capture the handshake. To capture a network's handshake we usually need to wait patiently for the network to disconnect and reconnect to a device in its range (If you are lucky, this happens at least once in a couple of days). To speed up the process we can send out a jamming signal called a "de-authentication signal". The signal (packet) forces the access point to disconnect from all devices for a brief second. All we have to do is send out a de-authentication packet and attempt to capture (scan for) a handshake at the same time.

Once we have captured the handshake, it's time to crack it.

In my case, the majority of Wifi passwords around me are phone numbers in the following format:

```plaintext
0XXXXXXXXX
```

Common knowledge tells me that the digit that usually comes after the initial 0 is 5. What is left for me to do is to comprise a word list of every possible number combination in 8 digits (the remainder of our template, minus the initial 05).

To achieve this I have used a simple Python script:

```python
import numpy as np

def main():
    digits = 8
    prefix = "05"
    possibilities = 8 ** digits

    template = np.random \
                 .choice(possibilities, size=possibilities, replace=False) \
                 .astype(str)

    lst = list(map(lambda n: n.zfill(digits), template))
    f_lst = [prefix + s for s in lst]
    
    with open('phonenumbers.txt', 'w') as file:
        file.write("\n".join(sorted(f_lst)))
    
    
if __name__ == "__main__": main()
```

I used Python’s NumPy to generate 8^8 possibilities and added the 2-digit static prefix to every line. The script took about 5 minutes to generate a list with 16,000,000 possibilities. Now that we have a word list (or a phone list) we can use tools like Hashcat along with our GPU to crack the captured handshake’s password.

Take a look at Hashcat and their quick guide on [cracking WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2). Using this and the wordlist we created, we should be able to crack a password in less than 10 minutes under the right conditions.

A final word of advice - do not leave this article thinking you have the knowledge to hack Wifi networks, leave this article knowing what you need to do to protect your own network from being hacked.