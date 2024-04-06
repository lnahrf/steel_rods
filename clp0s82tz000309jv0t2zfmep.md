---
title: "Demystifying electricity and power consumption for Arduino, embedded and IoT beginners"
seoTitle: "Arduino & IoT Power Guide"
seoDescription: "Unlock the secrets of electricity and power for Arduino and IoT projects, making complex concepts accessible for beginners"
datePublished: Thu Nov 16 2023 06:00:10 GMT+0000 (Coordinated Universal Time)
cuid: clp0s82tz000309jv0t2zfmep
slug: demystifying-electricity-and-power-consumption-for-arduino-embedded-and-iot-beginners
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/zP7X_B86xOg/upload/1e24d07d20dca92ea6c6040a5b0a3591.jpeg
tags: arduino, iot, embedded, electronics, energy

---

A decade ago, before I knew much about electricity - I had a hard time grasping the different concepts and terminology required to plan and wire even simple Arduino circuits. I tried learning and going over the dry, theoretical study materials over and over again. It took me a good couple of years of on-and-off self-teaching to firmly understand the basics. My goal in this article is to try and get you to the point I was in after things finally clicked. Hopefully, it will be faster for you than it was for me.

I will assume you have very basic knowledge of electronics (or none at all, that is also fine).

To understand electronic circuits, we need to first understand energy and therefore understand electricity. Bear with me through this, I will try to keep the explanation as short and as precise as possible.

*Please note that I am no electrical engineer! I might get some details wrong. If you notice a mistake, please send me a message and we will get it sorted out.*

### What is electricity?

> Electricity is the movement of electrons from point A to point B.

Electrons are particles that hold a negative charge, and they are present in all matter. Under specific circumstances, electrons can flow through conductive materials, such as different metals. When electrons flow through electronic components, they "power" them (for example, an LED will light up when power flows through it).

![](https://media.tenor.com/7HXxXDZdnEUAAAAd/palpatine.gif align="center")

This gives us the ability to engineer circuits that are combined of multiple components, these components are powered with an electrical current and are working together to achieve a specific goal.

### Direct Current vs Alternating Current

There are two types of electrical current we use in our daily lives. **Direct Current** or ***DC*** is the flow of electrons in one constant direction.

**Alternating Current** or ***AC*** is the same flow of electrons between A and B, but the direction of the current alternates in a certain frequency (based on whether the voltage that flows through the wire is positive or negative). For example, we can imagine electrons flowing through a wire from A to B.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699567033047/7dc088bf-821c-4a7f-b0ff-e079fa06c7c2.png align="center")

In AC, the direction of flow through a wire will alternate (switch between forward and backward, from A to B and then from B to A) based on the frequency of the current.

The frequency of the current is measured in Hertz (Hz) and it is usually between 50 and 60 cycles per second. The meaning of this is that under usual circumstances when using AC, the direction of the electrons flowing through the wire will alternate between 50 to 60 times per second.

The grid provides us with an AC-type current. This means the current coming out of your wall outlets is most likely alternating (although some home appliances might use DC, it is less common). Here is an example list of appliances in an average household and which current type they need to operate:

* Refrigerator - AC
    
* Washing Machine - AC
    
* Oven and Stove - AC
    
* Microwave - AC
    
* Air Conditioner - AC
    
* Computers and other electronic devices - DC
    

Electronic devices - such as computers, cell phones, routers and the like are most likely using DC that is internally converted (through a device called a converter) from the outlet's AC to the DC required to power the device. The clumsy power brick you carry around and use to power your laptop is a converter - a device designed to convert AC to DC (or vice-versa). The awkward block that connects to your wall outlet and charges your phone is also a converter.

*Quick tip: USB ports supply 5 volts DC as a standard.*

Now that we know what basic currents are there, let's move on to the meat and bones of electronic circuits - voltage and amperage.

### Voltage and Amperage

When electricity flows through a wire, we have two measurements that are important to take note of. The first measurement is Voltage. You have probably heard of voltage before, for example - the voltage I am receiving from my wall outlets is 220V (Volts).

Voltage is the potential amount of energy necessary to move one unit of charge (measured over time) between A and B (through a wire or circuit). In simpler terms:

> Voltage refers to the amount of energy necessary to move current from A to B.

Amperage (or amps for short), is the second measurement we need to take note of. Amps refer to the movement rate of electrons through a wire or circuit. If voltage is the energy required to move current through a wire, amps is the rate at which the current flows.

Multiplying voltage and amperage gives us power, measured in Watts (W). Joule's law tells us that:

```cpp
float power = voltage * amperage;
```

In this example, the variable 'power' is the power measured in Watts (W), 'voltage' in volts, and 'amperage' in amps. When multiplying voltage with amperage we get the power measurement of a circuit or component.

If my refrigerator requires 350 Watts to operate, we will need a power supply that can produce at least that amount of energy to run my fridge for a period of time.

Energy over time is measured in Watt Hours (Wh), to run my imaginary fridge for one hour we will need ***0.35kWh of energy*** (0.35 Kilowatt Hours).

### What does any of this have to do with Arduino and microcontrollers?

![](https://media.tenor.com/3OZu_n2PL-QAAAAd/star-wars-yoda.gif align="center")

Let's take into account a standard household and conjure a list of appliances and their approximate energy required to operate for an hour (this is an estimate, the exact measurements vary).

* Television - 0.2 kWh
    
* Refrigerator (large) - 1 kWh
    
* Washing Machine - 0.3 kWh
    
* Oven or Stove - 2 kWh
    
* Microwave - 0.5 kWh
    
* Air Conditioner - 1 kWh
    
* Laptop - 0.15 kWh
    
* Desktop - 0.3 kWh
    

According to this estimated list, for us to power all of these appliances simultaneously for an hour we will need a power supply (or battery) that can produce a minimum of 5.45 kWh.

```cpp
float kwh = 0.2 + 1 + 0.3 + 2 + 0.5 + 1 + 0.15 + 0.3; // 5.45 kWh
```

### Our household is a circuit

Let's convert our household analogy to electronic circuits using Arduino as an example. In this example, the household ***is*** our electronic circuit.

Imagine I have an Arduino, that is capable of supplying 5 volts with up to 800mA (milli-amperes) to appliances (or electronic components). Using the methodology of our previous example, we now know how to calculate the amount of components we will be able to power (potentially) using the Arduino without an external power supply.

### Calculating component power consumption

Each component you will use in your Arduino, embedded or IoT projects should have a specification document. The document should indicate its voltage and amperage requirements (or a range of).

For example, I searched for the specification document of the popular temperature and humidity sensor DHT11.

Skimming through the document I found the following table:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699732216074/da818ee7-efd9-42cb-8a8d-222b47a14189.png align="center")

In my calculation, I went for the average 3.3 volts and 0.2mA consumption, since I know the DHT11 operates properly when supplied with 3.3v.

```cpp
float power = 3.3 * (0.2 / 1000); // 0.00066 watts or 0.66 milli-watts
```

To calculate how many components we can hook up to an Arduino, we must know each component's power requirements. The power requirement of a component can be calculated using the following example (the values are made up).

```cpp
float power = volts * amps; // power in watts

// Note that 1W = 1000mW (milli-watts) and 1A = 1000mA (milli-amperes)
float power = 5 * 0.5; // 2.5w
// OR use 500 as in 500 mA
float power = 5 * 500; // 2500mW, converted to watts = 2500 / 1000 = 2.5w
```

Now that we know how to calculate the power consumption of each of our components, we can use these examples and calculate the total power consumption of our circuit.

```cpp
float component_a = 3.3 * 0.05; // 0.165w
float component_b = 3.3 * 0.2; // 0.66w
float component_c = 3.3 * 0.4; // 1.32w
float arduino = (5 * 50) / 1000; // 250mW / 1000 = 0.25w

float total = component_a + component_b + component_c + arduino; // 2.395w
```

Since our Arduino can supply up to 2.5w of power through the 5v pin, we should be in the clear (although in real-life scenarios, it's always important to add a 20%-40% margin to account for component efficiency and power fluctuations). So in reality, we would probably need an external power supply to power some of these components, or find power-efficient alternatives.

### Calculating the required battery capacity

We also know how to calculate the battery capacity we will need to run our circuit for a preiod of time (in watt-hours).

```cpp
float wh = volts * amps * hours;
/* 
wh = the watt-hours of energy our battery will need to supply.
volts = the voltage our power source will be supplying (our battery, in volts).
amps = the current our Arduino Uno draws, including all of
the components in our circuit (in amperes).
hours = the required duration of usage in hours. 
*/
```

In this example, our Arduino is drawing around 50mA to power its microcontroller, while being able to supply around 500mA-800mA in total. This means that our external components can draw up to 450mA in total for our circuit to be operable. Take a look at the following code snippet.

```cpp
float components_ma = 450;
float arduino_ma = 50;
float amps = (arduino_ma + components_ma) / 1000; // Convert milli-amperes to amperes
int volts = 5; // The voltage our battery will be supplying
int hours = 24;

float consumption = amps * volts; // 2.5 watts an hour
float wh = consumption * hours; // 60 watt hours
```

Remember that It is always best to add a 20%-40% safety margin when calculating power consumption, to account for real-life component efficiency (or inefficiency) and power fluctuations.

```cpp
float wh = constumpion * hours * 1.2; // 72 watt hour battery
```

The equation that converts watt-hours to milli-ampere-hours (which is the unit external batteries are usually measured in) is:

```cpp
float mah = wh * (1000 / volts); // 14400 mAh battery
```

14,400 mAh is quite a lot of energy (in the context of Arduino), but not all of our components will need to be powered for the whole 24 hours or at the same time. Our household logic applies, you won't run all of your appliances on full power 24/7 simultaneously, right?

Realistically, you will be able to implement power-saving methodologies that will reduce this estimated consumption significantly. But it is still wise to estimate the worst possible amount of energy our circuit might consume when working on Arduino and IoT projects.

Now that you understand some of the important terminology when it comes to Arduino power consumption, you should be able to use the examples given in this article and convert them to different devices, circuits, and requirements.

### Who am I?

My name is Lev, a self-proclaimed hacker and radio enthusiast, writing for my personal tech hub at [HackFM](https://hackfm.com/). I have been tinkering with computers for over a decade, and although I may not always be successful in putting them back together, I have gained some knowledge in the field. I currently work as a senior data engineer and I am always available for consulting or discussing anything tech-related. Want to write for [HackFM](https://hackfm.com/) or collaborate on a gritty technical article? Please let me know!