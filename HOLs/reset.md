Reset Instructions 
==================

The following commands will help you reset the so that you can repeat the lab steps.  

1. In the Node-RED interface on the NUC [http://your.nuc.ip.address:1880](http://your.nuc.ip.address:1880), delete the flows you created during the lab:
     - Flow 2 - The one where you blinked the LED
     - Flow 3 - Where we published the sensor data to Azure IoT Hubs

2. Make sure only one flow exists name "Flow 1".  Reset Flow 1 to the default Flow code:

```JSON
[
    {
        "id":"1dc1405c.26a96",
        "type":"mqtt-broker",
        "z":"",
        "broker":"localhost",
        "port":"1883",
        "clientid":"",
        "usetls":false,
        "verifyservercert":true,
        "compatmode":true,
        "keepalive":"60",
        "cleansession":true,
        "willTopic":"",
        "willQos":"0",
        "willRetain":null,
        "willPayload":"",
        "birthTopic":"",
        "birthQos":"0",
        "birthRetain":null,
        "birthPayload":""
    },
    {
        "id":"820c18b0.8f6928",
        "type":"function",
        "z":"700ef1be.41a15",
        "name":"changeColor",
        "func":"\nif (msg.payload >= 0 && msg.payload <= 78){\n    //red\n    msg.lcdColor = {r: 255, g: 0, b: 0};\n}\nelse if (msg.payload > 78 && msg.payload <= 156){\n    //orange\n    msg.lcdColor = {r: 255, g: 128, b: 0};\n}\nelse if (msg.payload > 156 && msg.payload <= 234){\n    //yellow\n    msg.lcdColor = {r: 255, g: 255, b: 0};\n}\nelse if (msg.payload > 234 && msg.payload <= 312){\n    //red-green\n    msg.lcdColor = {r: 128, g: 255, b: 0};\n}\nelse if (msg.payload > 312 && msg.payload <= 390){\n    //green\n    msg.lcdColor = {r: 0, g: 255, b: 0};\n}\nelse if (msg.payload > 390 && msg.payload <= 468){\n    //blue-green\n    msg.lcdColor = {r: 0, g: 255, b: 128};\n}\nelse if (msg.payload > 468 && msg.payload <= 546){\n    //green-blue\n    msg.lcdColor = {r: 0, g: 255, b: 255};\n}\nelse if (msg.payload > 546 && msg.payload <= 624){\n    //light-blue\n    msg.lcdColor = {r: 0, g: 128, b: 255};\n}\nelse if (msg.payload > 624 && msg.payload <= 702){\n    //blue\n    msg.lcdColor = {r: 0, g: 0, b: 255};\n}\nelse if (msg.payload > 702 && msg.payload <= 780){\n    //purple\n    msg.lcdColor = {r: 127, g: 0, b: 255};\n}\nelse if (msg.payload > 780 && msg.payload <= 858){\n    //pink-purple\n    msg.lcdColor = {r: 255, g: 0, b: 255};\n}\nelse if (msg.payload > 858 && msg.payload <= 936){\n    //pink\n    msg.lcdColor = {r: 255, g: 0, b: 127};\n}\nelse if (msg.payload > 936 && msg.payload <= 1023){\n    //gray\n    msg.lcdColor = {r: 128, g: 128, b: 128};\n}\n\n//don't display rotary value on lcd\nmsg.lcdCursor = {row: 1, column: 0};\nmsg.payload = '';\n\nreturn msg;",
        "outputs":1,
        "noerr":0,
        "x":326,
        "y":152,
        "wires":[["721d8dd6.b8cd54"]]
    },
    {
        "id":"2f4d2711.9881f8",
        "type":"exec",
        "z":"700ef1be.41a15",
        "command":"ifconfig eth0 | grep \"inet addr\" | awk -F: '{print$2}' | awk '{printf \"%s\", $1}'",
        "addpay":false,
        "append":"",
        "useSpawn":"",
        "name":"getIP",
        "x":323,
        "y":95.5,
        "wires":[["721d8dd6.b8cd54"],[],[]]
    },
    {
        "id":"f9520312.abfaa",
        "type":"inject",
        "z":"700ef1be.41a15",
        "name":"updateIP",
        "topic":"ip",
        "payload":"",
        "payloadType":"num",
        "repeat":"60",
        "crontab":"",
        "once":true,
        "x":158,
        "y":96,
        "wires":[["2f4d2711.9881f8"]]
    },
    {
        "id":"523b757b.206e6c",
        "type":"chart tag",
        "z":"700ef1be.41a15",
        "title":"Rotary",
        "chartType":"line",
        "dataSource":"Line",
        "units":"Raw",
        "min":"0",
        "max":"1023",
        "targetLow":"",
        "targetHigh":"",
        "priority":"",
        "sourcePriority":"1",
        "ttl":"3",
        "points":50,
        "x":325,
        "y":201,
        "wires":[["733fcf95.5a0d7"]]
    },
    {
        "id":"cd813fac.48dbd",
        "type":"chart tag",
        "z":"700ef1be.41a15",
        "title":"Rotary",
        "chartType":"gauge",
        "dataSource":"Gauge",
        "units":"",
        "min":"0",
        "max":"1023",
        "targetLow":"",
        "targetHigh":"",
        "priority":"",
        "sourcePriority":"2",
        "ttl":"3",
        "points":50,
        "x":326,
        "y":246,
        "wires":[["733fcf95.5a0d7"]]
    },
    {
        "id":"a80a1a67.91c2f8",
        "type":"upm-grove-rotary",
        "z":"700ef1be.41a15",
        "name":"",
        "platform":"512",
        "unit":"ABSRAW",
        "pin":"0",
        "interval":1000,
        "x":152,
        "y":183,
        "wires":[["820c18b0.8f6928","523b757b.206e6c","cd813fac.48dbd"]]
    },
    {
        "id":"721d8dd6.b8cd54",
        "type":"upm-grove-rgb-lcd",
        "z":"700ef1be.41a15",
        "name":"",
        "platform":"512",
        "r":0,"g":0,"b":255,
        "row":0,
        "column":0,
        "x":524,
        "y":82,
        "wires":[]
    },
    {
        "id":"733fcf95.5a0d7",
        "type":"mqtt out",
        "z":"700ef1be.41a15",
        "name":"Charts",
        "topic":"/sensors",
        "qos":"",
        "retain":"",
        "broker":"1dc1405c.26a96",
        "x":492,
        "y":221,
        "wires":[]
    }
]
```
3. In the NUC portal, on the "Packages" page:

    - Delete packagegroup-cloud-azure package
    - Delete IoT_Cloud repository

4. ssh (PuTTY, screens, ssh) into the NUC.  From the terminal run:   

```bash
rpm –e http://iotdk.intel.com/misc/iot_pub.key

npm uninstall node-red-contrib-os –g 

npm uninstall node-red-contrib-auzreiothubnode -g

systemctl restart node-red-experience
```

