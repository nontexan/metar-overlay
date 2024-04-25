# metar-overlay
Generate a graphic overlay for video webcam feed with the current METAR weather from airport AWOS

# AWOS Weather Source
If the local airport usese an AWOSNet weather station, then this scripting may be of use.

A list of airports using AWSONet weather stations:

`http://awosnet.com/default.php`

Example AWOS Webpage

`http://kaun.awosnet.com/`



The XML data for this page is updated every 5 minutes:

`curl http://kaun.awosnet.com/awiAwosNet.php`

Which will return data such as:

```
<?xml version="1.0" encoding="UTF-8"?>
<awosnet><airportName value="Auburn Municipal Airport" source="db" /><wmscrId value="kaun" source="db" /><faaId value="aun" source="db" /><phone_number value="530-888-8934" source="db" /><vhfFreq value="119.375" source="db" /><runwayA value="7" source="db" /><siteElevation value="1538" source="db" /><siteBpElevation value="\\\\" source="rmm" /><siteLatitude value="38.95483" source="db" /><siteLongitude value="-121.08172" source="db" /><siteDeviation value="16" source="db" />
<airportIdentifier value="KAUN" time="1714014616"></airportIdentifier>
<airTemperature value="14" time="1714014616"></airTemperature>
<altimeterSetting value="30.02" time="1714014616"></altimeterSetting>
<ceilometer value="OVC039" time="1714014616"></ceilometer>
<date value="25/04/2024" time="1714014616"></date>
<densityAltitude value="" time="1714014616"></densityAltitude>
<dewPoint value="7" time="1714014616"></dewPoint>
<METAR value="METAR KAUN 250310Z AUTO 18006KT 10SM OVC039 14/07 A3002 RMK A01" time="1714014616"></METAR>
<presentWeather value="" time="1714014616"></presentWeather>
<rainfallCurrentHour value="0" time="1714014616"></rainfallCurrentHour>
<relativeHumidity value="63" time="1714014616"></relativeHumidity>
<tenMinuteairTemperature value="14" time="1714014616"></tenMinuteairTemperature>
<tenMinutedewPoint value="7" time="1714014616"></tenMinutedewPoint>
<tenMinutevisibility value="10+" time="1714014616"></tenMinutevisibility>
<time value="03:10" time="1714014616"></time>
<twoMinutewindDirection value="170" time="1714014616"></twoMinutewindDirection>
<twoMinutewindSpeed value="6" time="1714014616"></twoMinutewindSpeed>
<variableWinds value="" time="1714014616"></variableWinds>
<windDirectionVariableString value="" time="1714014616"></windDirectionVariableString>
<windGust value="" time="1714014616"></windGust>
</awosnet>
```

Output just the METAR:
```
curl -s http://kaun.awosnet.com/awiAwosNet.php | grep -i metar | cut -d'"' -f2
METAR KAUN 250310Z AUTO 18006KT 10SM OVC039 14/07 A3002 RMK A01
```

## MacOS Tests
Converting the METAR text to an image graphic, Imagemagick is used to generate the graphic.

If this is not already installed on a Mac, consider using Home Brew to install it:

`brew install imagemagick`

## Using Imagemagick generating graphic

`convert -size  1700x150 -gravity center -background gray95 -fill black -font "Arial-Narrow" label:"METAR KAUN 071855Z AUTO 19006KT 10SM CLR 36/04 A3003 RMK A01" test.png`

## Shell script to parse AWOS METAR and generate graphics

```
cat create_AWOS_image.sh 
#/bin/bash

metar_text=$(curl -s http://kaun.awosnet.com/awiAwosNet.php | grep -i metar | cut -d'"' -f2) 
echo ${metar_text}
convert -size  1700x100 -gravity center -background gray95 -fill black -font "Arial-Narrow" label:"${metar_text}" KAUN.png
```


