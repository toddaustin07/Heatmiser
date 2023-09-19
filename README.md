# Heatmiser SmartThings Driver supporting Neohubs and Neostats
This is a SmartThings Edge Driver for Heatmiser Hubs (Neohubs) and Thermostats (Neostats).  As an Edge driver, it offers completely local processing and requires no other applications running on your LAN.  (The former DTH/Groovy-based driver required an additional bridge application running somewhere on your network)

## Supported States and Controls
* Temperature
* Operating State (Heat, Idle)
* Set mode (Auto, Standby)
* Set heating setpoint temperature and frost protection temperature (if enabled)
* Hold with duration
* Select active Profile
* Set all thermostats to Standby or Auto
* Set Away

### Pre-requisites
* SmartThings Hub
* Heatmiser Neohub (second generation)
* One or more Heatmiser Neostats
* neoHub API Access Token

Note that all the SmartThings and neoHub devices must reside on the same subnet of your network.  In other words, they must all be on 192.168.1.n, for example.

### Limitations
* The driver is currently in Beta with a limited number of testers to-date.  It has not been tested with configurations including multiple Neohubs.
* The driver supports Neostat thermostat devices only so far; support has not yet been added for other types of Heatmiser devices such as plugs, air sensors, etc, although that is being considered
* The driver supports heating system only at this time.  Combination Heating and Cooling systems (neoStat HC) can be supported in the future if requested.

## neoHub API Access Token Creation
Use the Heatmiser app and go to *Settings > API* and create a Token.  Be sure to save this somewhere safe, and where you can later copy and paste it into a SmartThings device Settings screen on your mobile device.

## Driver Installation
You should have an installed, configured, and operating Heatmiser system including Neohub and one or more Neostats.

### Driver Installation
The driver is currently available on my test channel [here](https://bestow-regional.api.smartthings.com/invite/Q1jP7BqnNNlL).  Enroll your hub to the channel and then choose "Heatmiser V0.1Beta" from the list of drivers to install.  This will result in the driver being installed to your SmartThings hub within 12 hours.

Once the driver is available on your SmartThings hub, the next time you use the SmartThings app to perform an *Add device / Scan for nearby devices*, your network will be scanned for any present Heatmiser Neohubs.  When found, a SmartThings device called "Heatmiser neoHub" will be created and found in the same room as your SmartThings hub device.

Note:  If no neoHub devices are found via the network scan, a "manual" version of the Heatmiser neoHub device will be created instead, which will require configuration of its IP address in device Settings (see neoHub device Configuration below).

### neoHub Device Configuration

Once your device has been created, you will first need to configure a couple options in the neoHub device Settings screen.  Open the device to the Controls screen and then tap the three vertical dot menu in the upper right corner and select "Settings".

It is recommended to edit these settings in the reverse order in which they appear - in other words, for automatically-found neoHub devices, configure the API Access Token last.  For manually created newHub devices, configure the Device Address last.

##### Device Address ('manual' hub devices only)
Here you configure the IP address of your neoHub device if it wasn't automatically found during network scan.

##### Neohub API Access Token
Here you provide the token you created above through the Heatmiser app

##### Refresh Frequency
Your neoHub will be polled on a periodic basis to get the latest data from all your thermostats.  Configure how often you want this to occur.  The default is every 60 seconds.  More frequent polling will require more driver activity on the SmartThings hub, more network activity, and more processing required for the neoHub, so take this into account when choosing your desired frequency.

##### Temperature Units
This is defaulted to Celsius, but can be changed to Fahrenheit if that is the temperature-unit at your location.

#### neoStat Device Creation
Once you complete configuration, return to the device Controls screen.  You should now see a Status of "Connected" if your API Access Token was accepted, and you should see a table of information appear in the Info section.

At this point you will also start to see thermostat devices created for each neoStat (or zone) that is known to your neoHub.  These new devices will also be found in the room where your SmartThings hub device is located.  They will have the name of "Neostat: xxxxxx", where "xxxxxx" is the name of the configured zone.

Within 15-20 seconds the data should get refreshed in each of the new thermostat devices and show the correct current states.



## Using the Devices

The devices provide the most critical information and functions for your system, but is not intended to replicate ALL functions of the Heatmiser app.  The purpose of having these devices integrated within SmartThings is to give the user a way to build automations that trigger based on thermostat states or that control your thermostats based on other SmartThings device states.

### neoStat Devices
The data shown on these devices will be refreshed at the frequency you configured in the neoHub device Settings.  If an action is taken such as changing the thermostat mode or setpoint temperature, then an additional data refresh will occur about 5 seconds after the action.  This will provide more immediate response to your requested actions.

In addition, at any time you can perform a swipe-down gesture on the Controls screen to force an immediate data refresh.

##### Temperature
This shows the current zone temperature.

##### State
This will show the current state of the system (Heating or Idle)

##### Thermostat Mode
This is a control to set the mode of the system to either Auto or Standby

##### Heating temperature
When in Auto mode, this control sets the heating setpoint temperature.  When in Standby mode, and if frost protection is enabled for your zone, this control can be used to adjust the frost setpoint temperature in the range of 7 to 17 degrees Celsius.

Note that if in Standby mode and frost protection is disabled for your zone, this value will automatically be set to 4 degrees Celsius, and changing the value will have to affect on the thermostat.

##### Hold
Tapping the button will request Hold to be enabled; tapping again will disable.  The hold duration requested is configured below.

##### Hold Duration
This sets the hours and minutes for the requested Hold.  Format must be hh:mm / h:mm / hh:m / h:m, with leading 0's optional (e.g. 01:15 or 1:15), where 'hh' (hours) is 00-23 and 'mm' (minutes) is 00-59.

##### Set Thermostat Profile
A list of available Profiles available on your system will be displayed and can be selected for activation.  Note that the Profile must be enabled for the zone requested.

##### Status
This field generally show the last time the data has been updated, or other messages if the neoHub has not been connected to.

##### Info
This is a table of additional useful information to monitor your zone, such as the current active profile, away state, available modes, device battery state, and zone name


### neoHub Device

The user can perform a swipe-down gesture on the Controls screen to force a re-connection with the neoHub if he/she expects the connection has been lost.  Generally, the Status field should be used to monitor the connection state.

The controls available on this device (Standby All, Away, Set Profile) are intended to be system-wide actions to be performed across all thermostats configured for the neoHub.  Thermostat-specific actions should be performed from the respective neoStat device Controls screen.

##### Status
Generally this should show "Connected" in normal operation, but if the connection is lost with your neoHub it will show other messages.

##### Standy All
This button allows you to perform a "one-shot" setting of all thermostats to either Standby mode ON or Standby mode OFF.

##### Away
This button is used to enable or disable Away status for your system.  Note that the "HUB_AWAY" item in the Info table will also show the current Away status.

##### Set Thermostat Profile
A list of available Profiles available on your system will be displayed and can be selected for activation.  Note that the Profile will only apply to the zones it is configured for.

##### Info
This is a table of additional useful information related to your neoHub configuration.

## Additional Information
### Problems
* If your neoHub device is not found during network scan, be sure it is on the same subnet as your SmartThings hub.  The discovery method used is mDNS; neoHubs should be making themselves known on the network via this method.
* If your neoHub device is not connecting to your neoHub, it is most likely due to an invalid API Access Token.  Check that you have entered it correctly (copy/pasting is the safest way to do it), and that there are no extraneous leading or trailing spaces.
* If you seem to be missing some of your neoStat devices getting created, try swipping down on the neoHub Controls screen, which will force a reconnection to the neoHub.  During this process, the list of supported zones is retrieved and if any devices are found to be missing, they will be created.

### Deleting Heatmiser Driver Devices
* If you manually delete any of your SmartThings neoStat devices, they will get re-created the next time the driver connects to the neoHub (this can be forced to happen at any time by swiping down on the neoHub device Controls screen).

* If you want to permanently delete all your Heatmiser devices, simply delete the neoHub device, and all its associated neoStat devices will also be deleted automatically (they are 'child' devices).
