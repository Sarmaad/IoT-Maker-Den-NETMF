Documentation in progress.


### IoT-Maker-Den-NETMF

[Complete Maker Den Lab Guide](https://github.com/MakerDen/IoT-Maker-Den-NETMF/blob/master/MakerDen/Lab%20Code/IoT%20Maker%20Den%20v2.0.pdf)

The Maker Den IoT Framework provides a pluggable foundation to support sensors, actuators, data serialisation, communications, and command and control. 


![Alt text](https://github.com/MakerDen/IoT-Maker-Den-NETMF/blob/master/MakerDen/Lab%20Code/Maker%20Den%20IoT%20Framework.jpg)


## Extensible/pluggable framework supporting

Sensors

* Physical: Light, Sound, Temperature (onewire), Servo
* Virtual: Memory Usage, Diagnostics
* Sensor data serialised to a JSON schema

Actuators

* RGB, RGB PWM, Piezo, Relay, NeoPixel (strips, rings and grids)

Command and Control

* Control relays, start neo pixels etc via comms layer

Communications
* Pluggable – currently implemented on MQTT (MQTT Server running on Azure)

Supported and Tested
* Netduino 2 Plus and Gadgeteer
* Supports Visual Studio 2012 and 2013
 

## Programming Models

    using Glovebox.MicroFramework.Sensors;
    using Glovebox.Netduino.Actuators;
    using Glovebox.Netduino.Sensors;
    using Microsoft.SPOT;
    using SecretLabs.NETMF.Hardware.NetduinoPlus;
    using System.Threading;

    namespace MakerDen {
        public class Program : MakerBaseIoT  {
            public static void Main() {
                // main code marker
                
                //Replace the "emul" which is the name of the device with a unique 3 to 5 character name
                //use your initials or something similar.  This code will be visible on the IoT Dashboard
                StartNetworkServices("emul", true);

                using (SensorTemp temp = new SensorTemp(Pins.GPIO_PIN_D8, 10000, "temp01"))
                using (SensorLight light = new SensorLight(AnalogChannels.ANALOG_PIN_A0, 1000, "light01"))
                using (rgb = new RgbLed(Pins.GPIO_PIN_D3, Pins.GPIO_PIN_D5, Pins.GPIO_PIN_D6, "rgb01")) {
                
                    temp.OnBeforeMeasurement += OnBeforeMeasure;
                    temp.OnAfterMeasurement += OnMeasureCompleted;
                    light.OnBeforeMeasurement += OnBeforeMeasure;
                    light.OnAfterMeasurement += OnMeasureCompleted;
                    
                    Thread.Sleep(Timeout.Infinite);
                }
            }
        }
    }




![Alt text](https://github.com/MakerDen/IoT-Maker-Den-NETMF/blob/master/MakerDen/Lab%20Code/DevModel2.JPG)
