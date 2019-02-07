## Sensor providing Public Transport information from [OVapi](http://www.ovapi.nl) in Home Assistant

This is a sensor for Home Assistant and it will retrieve departure information of a particular stop. The sensor returns the first upcomming departure.

### Install:
- Copy the ovapi.py file to: /config/custom_components/sensor/
- Add the content below to configuration.yaml:

```yaml
sensor:
  - platform: ovapi
    name: Tram_6
    stop_code: '9505'
    route_code: '32009505'
```
Sensor configuration options
- **stop_code** *(Required, See 1)*
- **timing_point_code** *(Required, See 1)*
- **route_code** - A stop always has two routes, a route forth and back. With this code you can configure the route destination.
- **show_future_departures** *(Optional)* - The sensor always creates one sensor in Hass, this property can be configured with a value of 2-5. If configured the component will create the number of sensors configured here. These sensors contain future departments together with theire delay if applicable.
- **line_filter** *(Optional)* - You might bump into the fact that there are multiple lines are traveling trought your targeted stop, with this property you can filter all passes with the line number you want.

1. Either one of these are required for the sensor to function!


### To find the stop_code (stopareacode) refer to the JSON response of: [v0.ovapi.nl](http://v0.ovapi.nl/stopareacode)
I've used the building JSON parser from Firefox, the search input is on the top right.

- Search in the response with a keyword of the stop or the line you want, eg: kastelenring
- The result:
```json
9505:    <-- is the stop
  TimingPointName "Kastelenring"
```
- Browse the following Url: http://v0.ovapi.nl/stopareacode/9505 (replace 9505 with the code you've found!)
- Minimize the child keys beneath your stop_code, two keys should be returned (these are the route fourth and back).
- Open one of the keys and browse to stop_code/route_code/Passes
- This key holds all the stops currently available (this updates frequently)
- Open one of the stops
- Look for the key 'DestinationName50', this should hold the destination that you want. If this is the wrong way, then you should close the JSON output and open the other route code key.
- Note the route_code and the stop_code and place these values in the sensor configuration.

### Note and credits
- [Petro](https://community.home-assistant.io/u/petro/summary) - For extensive help at coding the template.
- [Robban](https://github.com/Kane610) - A lot of basic help with the Python code.
- [Danito](https://github.com/danito/HA-Config/blob/master/custom_components/sensor/stib.py) - I started with his script, learned a lot of it)
- [pippyn](https://github.com/pippyn) - Huge contributions and a lot of bugfixes, thanks mate!

Above example wil only show the first upcomming departure, for more options please see: [Using the sensor](https://github.com/Paul-dH/Home-Assisant-Sensor-OvApi/blob/master/resources/using_the_sensor.md)

