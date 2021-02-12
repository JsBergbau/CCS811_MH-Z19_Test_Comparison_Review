# Accuracy of CCS811 air quality sensor compared to two NDIR Sensors: MH-Z19 and CO2Meter

I was curious how accurate the CCS811 air quality sensor is. So I compared it to two NDIR CO2 sensors.

## MH-Z19B CO2 Sensor vs TFA AirCO2ntrol / CO2 Meter vs CCS811 Test

As NDIR sensors I used the quite cheap (about 20 $) MH-Z19B sensor, Link https://www.winsen-sensor.com/d/files/infrared-gas-sensor/mh-z19b-co2-ver1_0.pdf
Because measurement differed a lot compared to CCS811 I added another NDIR sensor, just to make sure this cheap sensor works as expected. So this is also a small review of the MH-Z19B CO2 sensor.
The second NDIR CO2 meter is https://www.co2meter.com/products/co2mini-co2-indoor-air-quality-monitor branded from TFA as AirCO2ntrol https://www.tfa-dostmann.de/produkt/co2-monitor-airco2ntrol-mini-31-5006/

Both sensors were calibrated to 400 ppm air. But calibrating the TFA CO2 meter is not so easy. It was calibrated a few month ago by turning auto calibration on while beeing outside and then disabling auto calibration again.
MH-Z19B was calibrated a few weeks ago with fresh air. Autocalibration was set to off.

Reading of MH-Z19B was done in Node-RED via node-red-contrib-mh-z19-co2sensor https://flows.nodered.org/node/node-red-contrib-mh-z19-co2sensor
CO2 Meter was read by https://github.com/JsBergbau/TFACO2AirCO2ntrol_CO2Meter 

CCS811 was also read via Node-RED https://flows.nodered.org/node/node-red-contrib-ccs811-airquality-sensor
Baseline for calibration was set every minute so results are comparable and the automatic baselinecalibration can't affect the results.
TVOC and eCO2 are linearly correlated. Restistance is different for every sensor. Here you have to multiply resistance by 250 to get the result in Ohms. For a better visualization it was devided by 250.

Values were stored in InfluxDB and visualized with Grafana.

Now to the results.

First comparision:

<a href="https://raw.githubusercontent.com/JsBergbau/CCS811_MH-Z19_Test_Comparison_Review/main/1.jpg"><img width="850" src="https://raw.githubusercontent.com/JsBergbau/CCS811_MH-Z19_Test_Comparison_Review/main/1.jpg" alt="CCS811 compared to MH-Z19B compared to NDIR CO2Meter Figure 1" title="CCS811 compared to MH-Z19B compared to NDIR CO2Meter Figure 1" /></a>


As you can see MH-Z19B and TFA AirCO2ntrol are quite parallel to each other.

Despite constantly falling CO2 concentrations between 02:00 and 8:00 CCS811 shows quite constant values and even shows rising values. 
Then around 8:00 there is a high peak. After that values constantly fall despite CO2 values rise up. 

Next sample:

<a href="https://raw.githubusercontent.com/JsBergbau/CCS811_MH-Z19_Test_Comparison_Review/main/2.jpg"><img width="850" src="https://raw.githubusercontent.com/JsBergbau/CCS811_MH-Z19_Test_Comparison_Review/main/2.jpg" alt="CCS811 compared to MH-Z19B compared to NDIR CO2Meter Figure 2" title="CCS811 compared to MH-Z19B compared to NDIR CO2Meter Figure 2" /></a>

In the middle of the night, when I was sleeping, CCS811 showed a high TVOC concentration. 
Afterwars while CO2 concentration is quite slowly dropping, CCS811 shows a high drop of TVOC. 
Then around 16:00 while CO2 is quite constant, CCS811 shows two massive peaks in between a low concentration of TVOC. 
Then at 20:00 when there is a high peak in CO2 concentration and CCS811 shows nothing at all.

## Other metal oxide gas sensors

There are other metal oxide gas sensors, like the Bosch BME680. However that sensor shows higher TVOC values with rising temperature, see https://www.jaredwolff.com/finding-the-best-tvoc-sensor-ccs811-vs-bme680-vs-sgp30/

So even other TVOC sensor can't be recommended at the moment.

These metal oxide sensors also have to be calibrated regularly. At factory setting the CCS811 is self calibrating by taking the lowest measured TVOC value as 0 TVOC / 400 ppm. Even if you ventilate well
you barely reach that value. Especially in winter ventilating until there is completely fresh air with 0 TVOC / 400 ppm is a huge waste of energy. Depending on the outside temperature, window size and wind 
it can take hours until you reach completely fresh air with 400 ppm.

You can't disable automatic calibration on CCS811 since Firmware 2.x, so setting the baseline calibration register like every minute is the only way to go. 
However sensor resistance drifts over time and even with burned in sensors manufacturer recommends saving the baseline every 5 to 7 days, which means calibration every 5 to 7 days.

The MH-Z19B is still after months without calibration quite accurate. 

## Conclusion 

It is still not very clear what the CCS811 measures. Since I have no explanation for these spikes I can't recommend the CCS811 sensor.
If you you have an explantion, please open an issue and share your results.


