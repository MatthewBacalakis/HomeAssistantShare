# Summary

I recently added the [Google Maps Travel Time](https://www.home-assistant.io/integrations/google_travel_time/) integration to my Home Assistant instance so I could display travel times in my dashboard.  Along with the travel time I wanted to indicate how many minutes of delay traffic was causing.  

After playing around with this I discovered:
- The state of the integration's sensor is the travel time accounting for traffic delays if available, and the travel time without accounting for traffic if not.  When using the sensor state value you don't know which was used. 
- The sensor exposes `duration` and `duration_in_traffic` attributes but always populates them with the text result (`x day(s) y hour(s)`, `x hour(s) y min(s)`, or `x min(s)`) even though the google API returns a response like `{ "text": "3 hours 55 mins", "value": 14078 }`

So, it appears the exact value in minutes not accounting for traffic is unavailable to Home Assistant for my purposes.  I therefore created this template sensor to convert the text value we have to minutes and compare it to the sensor state so it can display the difference between the two.  Accuracy will likely suffer when durations are on the order of days due to rounding of the text value to days and hours. 

# Usage
Create a [Template Sensor Helper](https://www.home-assistant.io/integrations/template/) with this template.  Replace `TRAVEL_SENSOR_ID` with the id of your Google Maps Travel Time Sensor. I set the device class to `duration` and unit of measurement to `min` so it's styled the same as the integration's sensor. 

