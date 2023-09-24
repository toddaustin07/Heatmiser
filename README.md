# heatmiser SmartThings Driver
This is a SmartThings Edge Driver for heatmiser Hubs (neoHubs) and devices - specifically supporting thermostats (neoStat), plugs (neoPlug), and air (neoAir) device types. 

As an Edge driver, this offers completely local processing and requires no other applications running on your LAN (as compared to the former DTH/Groovy-based driver, which required an additional bridge application running somewhere on your network).

If you get value from using this driver, please consider making a donation to the effort!
  * [**Buy-me-a-coffee**](https://www.buymeacoffee.com/taustin)

## Supported States and Controls
#### neoStat
* Current temperature
* Operating State (Heat, Idle)
* Set mode (Auto, Standby)
* Set heating setpoint temperature and frost protection temperature (if enabled)
* Hold with duration
* Select active Profile
* Set all thermostats to Standby or Auto
* Set Away
#### neoStat configured to be Timer
* On/off switch
#### neoPlug
* On/off switch
#### neoAir
* Current temperature

### Pre-requisites
* SmartThings Hub
* heatmiser neoHub (second generation)
* One or more heatmiser neoStats
* neoHub API Access Token

Note that the SmartThings hub and heatmiser neoHub devices must reside on the same subnet of your network.  In other words, they must all be on 192.168.1.n, for example.

### Limitations
* The driver is currently in Beta with a limited number of testers to-date.  It has not been tested with configurations including multiple neoHubs.
* Not all heatmiser device types are supported (see above)
* The driver supports heating system thermostats only at this time.  Combination Heating and Cooling systems (neoStat HC) can be supported in the future if requested.

## neoHub API Access Token Creation
1. Ensure your mobile device is on the same subnet as your neoHub, and use the heatmiser app and go to *Settings > API* and create a Token.  
2. Be sure to save the generated Token somewhere safe (use the copy icon next to the generated Token), and where you can later copy and paste it into a SmartThings device Settings screen on your mobile device. 

#### Known issues
* The Token may disappear from the heatmiser app after you create it; a reboot of the neoHub may fix this
* A bug in the heatmiser app exists that may prevent iOS mobile devices from creating the Token

## Driver Installation
Before proceding, you should have an installed, configured, and operating heatmiser system including a neoHub and one or more neoStats.

### Driver Installation
The driver is currently available on my test channel [here](https://bestow-regional.api.smartthings.com/invite/Q1jP7BqnNNlL).  Enroll your hub to the channel and then choose "heatmiser v0.1Beta" from the list of drivers to install.  This will result in the driver being installed to your SmartThings hub within 12 hours.

Once the driver is available on your SmartThings hub, the next time you use the SmartThings app to perform an *Add device / Scan for nearby devices*, your network will be scanned for any present heatmiser neoHubs.  When found, a SmartThings device called "Heatmiser neoHub" will be created and found in the same room as your SmartThings hub device.

Note:  If no neoHub devices are found via the network scan, a "manual" version of the Heatmiser neoHub device will be created instead, which will require configuration of its IP address in device Settings (see neoHub device Configuration below).

### neoHub Device Configuration

Once your device has been created, you will first need to configure a couple options in the neoHub device Settings screen.  Open the device to the Controls screen and then tap the three vertical dot menu in the upper right corner and select "Settings".

**It is recommended to edit these settings *in the reverse order in which they appear*** - in other words, for automatically-found neoHub devices, configure the API Access Token last.  For manually created newHub devices, configure the Device Address last.

##### Device Address ('manually-created' hub devices only)
Here you configure the IP address of your neoHub device if it wasn't automatically found during network scan.

##### Neohub API Access Token
Here you provide the token you created above through the heatmiser app

##### Refresh Frequency
Your neoHub will be polled on a periodic basis to get the latest data from all your thermostats.  Configure how often you want this to occur.  The default is every 60 seconds.  More frequent polling will require more driver activity on the SmartThings hub, more network activity, and more processing required for the neoHub, so take this into account when choosing your desired frequency.

##### Temperature Units
This is defaulted to Celsius, but can be changed to Fahrenheit if that is the temperature-unit at your location.

#### Child Device Creation
Once you complete configuration, return to the device Controls screen.  You should now see a Status of "Connected" if your API Access Token was accepted, and you should see a table of information appear in the Info section.

At this point you will also start to see devices created for each zone (neoStat/neoPlug/neoAir) that is known to your neoHub.  These new devices will also be found in the room where your SmartThings hub device is located.  They will have the name of "neoStat: xxxxxx", where "xxxxxx" is the name of the configured zone (or "neoPlug: xxxxxx", or "neoAir: xxxxxx").

Within 15-20 seconds the data should get refreshed in each of the new and show the correct current states.

### neoStat Device Settings Options
A neoStat device can optionally be configured as a Timer, so there is a Settings option in the SmartThings neoStat devices that allows you to configure this mode so that the device displays an on/off switch rather than thermostat controls.  This should only be selected if you have first configured your actual neoStat as a Timer using the heatmiser app.  

Note that it may take a few moments for the SmartThings device to be fully updated in the SmartThings app with the new switch-only device profile.

## Using the Devices

The devices provide the most critical information and functions for your system, but are not intended to replicate ALL functions of the heatmiser app.  The purpose of having these devices integrated within SmartThings is to give the user a way to build automations that trigger based on heatmiser device states or that control your heatmiser devices based on other SmartThings device states.

### neoStat Devices
The data shown on these devices will be refreshed at the frequency you configured in the neoHub device Settings.  

If the neoStat device was configured through the heatmiser app to be a Timer, you should have set the corresponding SmartThings neoStat device Settings option as described earlier.  In Timer mode, the device will only include a switch control and a status field.

If an action is taken such as changing the thermostat mode or setpoint temperature, then an additional data refresh will occur about 5 seconds after the action.  This will provide more immediate response to your requested actions.

In addition, at any time you can perform a swipe-down gesture on the Controls screen to cause an *immediate* data refresh.

#### neoStat device Control screen fields in thermostat mode

##### Temperature
This shows the current zone temperature.

##### State
This will show the current state of the system (Heating or Idle)

##### Thermostat Mode
This is a control to set the mode of the system to either Auto or Standby

##### Heating temperature
When in Auto mode, this control sets the heating setpoint temperature.  

When in Standby mode, if frost protection is **enabled** for your zone, this control can be used to adjust the frost setpoint temperature in the range of 7 to 17 degrees Celsius.

If in Standby mode, and frost protection is **disabled** for your zone, this value will automatically be set to 4 degrees Celsius, and changing the value will have no affect on the thermostat.

##### Hold
Tapping this button will request Hold to be enabled; tapping again will disable.  The hold duration requested is configured below.

##### Hold Duration
This sets the hours and minutes for the requested Hold.  Format must be hh:mm / h:mm / hh:m / h:m, with leading 0's optional (e.g. 01:15 or 1:15), where 'hh' (hours) is 00-23 and 'mm' (minutes) is 00-59.

##### Set Thermostat Profile
A list of available Profiles available on your system will be displayed and can be selected for activation.    The Profiles displayed are those defined at the time the driver and neoHub connected.  If you subsequently create Profiles and they are not listed, then force a reconnection/reinitialization with the hub by swiping down on the **neoHub** Controls screen.

Keep in mind that the Profile you select must be enabled for the zone requested.

*Note that you will have to tolerate the "Tap to choose a favorite" wording here - this is a stock SmartThings capability and that label cannot be tailored.*

##### Status
This field generally shows the last time the neoStat data has been updated, or may contain other messages if the neoHub has not been connected to.

##### Info
This is a table of additional useful information to monitor your zone, such as the currently-active profile, away state, available modes, device battery state, and zone name

### neoPlug Devices
This is a simple SmartThings switch device, providing on/off control for the plug. 

### neoAir Devices
These SmartThings devices display the current temperature from the device.  The data shown on these devices will be refreshed at the frequency you configured in the neoHub device Settings.

### neoHub Devices
The user can perform a swipe-down gesture on the neoHub Controls screen to force a re-initialization with the neoHub, updating system configuration including hub configuration, profiles, frost setpoints, and configured zones.  If the user has created new profiles or zones, then this re-initialization will refresh the driver data and create any new devices if needed.

Generally, the Status field should be used to monitor the connection state.

Note that if the connection to the hub is lost at any time, there will be an automatic connection retry every 15 seconds, so normally disconnects should eventually recover without user action.

The thermostat-related controls provided on the neoHub device (Standby All, Away, Set Profile) are intended to be system-wide actions to be performed across all thermostats configured for the neoHub.  Zone-specific actions should be performed from the respective neoStat device Controls screen.

#### neoHub device Control screen fields

##### Status
Generally this should show "Connected" in normal operation, but if the connection is lost with your neoHub it will show other messages.

##### Standy All
This button allows you to perform a "one-shot" setting of all thermostats to either Standby mode ON or Standby mode OFF (Auto).

##### Away
This button is used to enable or disable Away status for your system.  Note that the "HUB_AWAY" item in the Info table will also show the current Away status.

##### Set Thermostat Profile
A list of available Profiles available on your system will be displayed and can be selected for activation.    The Profiles displayed are those defined at the time the driver and neoHub connected.  If you subsequently create new Profiles and they are not listed, then force a reinitialization with the newHub by swiping down on the neoHub device Controls screen.

Keep in mind that the Profile you select will be enabled only for the zones for which it was configured.

*Note that you will have to tolerate the "Tap to choose a favorite" wording here - this is a stock SmartThings capability and that label cannot be tailored.*

##### Info
This is a table of additional useful information related to your neoHub configuration.

## Additional Information
### Automations
The elements in all these devices are available in automations in the IF and THEN seconds of Routines or Rules.  

#### Notes
* Thermostat Operating State:  Use only the 'Heating' or 'Idle' values for IF conditions
* For selecting a Profile, use the "Play a favorite" option in Routine config screens, or the mediaPresets capability in Rules.  For the value to set, use the numeric heatmiser PROFILE_ID, which will be displayed in the Info table 'ACTIVE_PROFILE' element.  Do not use the Profile name.

### Problems
* If your heatmiser iOS app is not letting you create an API access token, you may have to borrow an Android device from a friend!
* If your neoHub device is not found during network scan, be sure it is on the same subnet as your SmartThings hub.  The discovery method used is mDNS; neoHubs should be making themselves known on the network via this method.
* If your SmartThings neoHub device is not connecting to your neoHub, it is most likely due to an invalid API Access Token.  Check that you have entered it correctly (copy/pasting is the safest way to do it), and that there are no extraneous leading or trailing spaces.
* If you seem to be missing some of your neoStat devices getting created, try swipping down on the neoHub Controls screen, which will force a reinitialization with the neoHub.  During this process, the list of supported zones is retrieved and if any devices are found to be missing, they will be created.

### Deleting heatmiser Driver Devices
* If you manually delete any of your SmartThings neoStat, neoPlug, or neoAir devices, they will get re-created the next time the driver connects to the neoHub (this can be forced to happen at any time by swiping down on the neoHub device Controls screen).

* If you want to permanently delete all your heatmiser devices, simply delete the neoHub device, and all its associated devices will also be deleted automatically (they are 'child' devices).
