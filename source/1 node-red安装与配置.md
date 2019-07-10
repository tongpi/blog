# 1、安装node-red（如果本机没有安装过）

> 参考：[Node-Red 官方文档](https://nodered.org/docs/getting-started/installation)
>
> 说明：
>
> 内外MQTT服务器:192.168.15.100 端口:1883
>
> 公网MQTT服务器:iot.eclipse.org 端口:1883

	## 第一步  安装 nodejs

下载：	https://nodejs.org/dist/v8.11.3/node-v8.11.3-x64.msi

安装：http://www.runoob.com/nodejs/nodejs-install-setup.html

验证：打开命令行窗口，输入 node --version & npm --version ，回车,看到如下输出，表示node环境已经安装好。

```shell
d:\node --version & npm --version 
v8.9.0
5.5.1
```

## 第二步  安装 node-red

> 打开命令行窗口，输入: npm install -g --unsafe-perm node-red，回车

###  运行 node-red

> 打开命令行窗口，输入: node-red  ,回车

### 打开 node-red 编辑器

> 在浏览器中输入：http://127.0.0.1:1880   可以看到如下页面：
>
> ![](D:\Personal\Desktop\node-red\images\editor.png)

# 2、使用 node-red

- 设计流

> 在node-red 编辑器中：
>
> 从左侧【输入】组中拖一个输入节点到中间流程1的画布上，如：inject
>
> 从左侧【输出】组中拖一个输出节点到中间流程1的画布上，如：debug
>
> 拖动画布上inject节点右侧的小方框，画一条线连接到debug左侧的小方框，放开鼠标，完成流定义.

- 部署流

  > 点击编辑器窗口右上角的  部署  按钮，部署流到服务器上。有为部署的更改，部署按钮呈现为  红色

  ![](D:\Personal\Desktop\node-red\images\deploy.png)

- 调试流

  > Debug的输出默认会显示到右侧的【调试窗口】选项卡中。

  > 部署后，点击流图中inject节点左侧的方框，可以在右侧的【调试窗口】选项卡中看到输出了一个时间戳。
  >
  > 

- 导入流

  > 下面的流设计中，包含了MQTT IN 、MQTT OUT、inject、debug节点构成的三个流。
  >
  > 复制下面的流设计到剪切板，然后到node-reg编辑器页面，点击CTRL+i 打开导入对话框，粘贴后点击对话框右下角红色的[导入]按钮，既可以将流导入到流设计器中，然后就可以部署流，测试流。

  ```javascript
  [
      {
          "id": "c16fb04d.9f481",
          "type": "worldmap",
          "z": "6ac85c37.86cd94",
          "name": "",
          "lat": "38",
          "lon": "120",
          "zoom": "",
          "layer": "OSM",
          "cluster": "",
          "maxage": "",
          "usermenu": "show",
          "layers": "show",
          "panit": "true",
          "x": 770,
          "y": 220,
          "wires": []
      },
      {
          "id": "c32606fa.45cb38",
          "type": "inject",
          "z": "6ac85c37.86cd94",
          "name": "",
          "topic": "",
          "payload": "",
          "payloadType": "str",
          "repeat": "",
          "crontab": "",
          "once": false,
          "onceDelay": "",
          "x": 250,
          "y": 200,
          "wires": [
              [
                  "a9e482ae.e2e9"
              ]
          ]
      },
      {
          "id": "a9e482ae.e2e9",
          "type": "function",
          "z": "6ac85c37.86cd94",
          "name": "Add person to map",
          "func": "var geo = { \"type\": \"FeatureCollection\",\n\"features\": [\n  {\n    \"type\": \"Feature\",\n    \"properties\": {\n      \"color\": \"rgb(0,0,255)\",\n      \"roofColor\": \"rgb(128,128,255)\",\n      \"height\": 20,\n      \"minHeight\": 0\n    },\n    \"geometry\": {\n      \"type\": \"Polygon\",\n      \"coordinates\": [\n        [\n        [117.356221,37.048611],\n        [119.356039,38 + Math.random()],\n        [119.355765,39 + Math.random()],\n        [119.355937,39 + Math.random()],\n        [117.356221,37.048611]\n        ]\n      ]\n    }\n  }\n]\n}\nvar m = {overlay:\"Golf Clubhouse\", geojson:geo, fit:true};\nmsg.payload = {command:{map:m, lat:37.0484, lon:117.3558}};\nreturn msg;",
          "outputs": 1,
          "noerr": 0,
          "x": 490,
          "y": 200,
          "wires": [
              [
                  "c16fb04d.9f481"
              ]
          ]
      },
      {
          "id": "cf043790.be4598",
          "type": "comment",
          "z": "6ac85c37.86cd94",
          "name": "http://(your-server-ip):1880/worldmap  或者 ctrl+shift+md打开地图",
          "info": "Adds a map at http://(your-server-ip):1880/worldmap. \n\nThe `function` node creates an object with some basic properties required to add to a map.",
          "x": 570,
          "y": 20,
          "wires": []
      },
      {
          "id": "ce262b25.8fb818",
          "type": "serial in",
          "z": "6ac85c37.86cd94",
          "name": "",
          "serial": "a70cd2b3.3fa06",
          "x": 150,
          "y": 440,
          "wires": [
              [
                  "bd386d41.de885"
              ]
          ]
      },
      {
          "id": "7d768b72.4f46e4",
          "type": "debug",
          "z": "6ac85c37.86cd94",
          "name": "",
          "active": true,
          "tosidebar": true,
          "console": false,
          "tostatus": false,
          "complete": "payload",
          "x": 730,
          "y": 100,
          "wires": []
      },
      {
          "id": "fe69ea4d.151248",
          "type": "function",
          "z": "6ac85c37.86cd94",
          "name": "",
          "func": "var nmea=global.get(\"nmea\");\nvar data = nmea.parseNmeaSentence(msg.payload);\nmsg.payload=data\nreturn msg;",
          "outputs": 1,
          "noerr": 0,
          "x": 290,
          "y": 380,
          "wires": [
              [
                  "d2cf5607.08bdb8"
              ]
          ]
      },
      {
          "id": "91a85599.1b9728",
          "type": "debug",
          "z": "6ac85c37.86cd94",
          "name": "",
          "active": true,
          "tosidebar": true,
          "console": false,
          "tostatus": false,
          "complete": "payload",
          "x": 750,
          "y": 580,
          "wires": []
      },
      {
          "id": "4f4ae634.f8af28",
          "type": "change",
          "z": "6ac85c37.86cd94",
          "name": "",
          "rules": [
              {
                  "t": "set",
                  "p": "payload",
                  "pt": "msg",
                  "to": "WIND",
                  "tot": "str"
              }
          ],
          "action": "",
          "property": "",
          "from": "",
          "to": "",
          "reg": false,
          "x": 450,
          "y": 600,
          "wires": [
              []
          ]
      },
      {
          "id": "bd386d41.de885",
          "type": "switch",
          "z": "6ac85c37.86cd94",
          "name": "",
          "property": "payload",
          "propertyType": "msg",
          "rules": [
              {
                  "t": "cont",
                  "v": "$GPRMC",
                  "vt": "str"
              },
              {
                  "t": "cont",
                  "v": "$WIMWD",
                  "vt": "str"
              }
          ],
          "checkall": "true",
          "repair": false,
          "outputs": 2,
          "x": 290,
          "y": 540,
          "wires": [
              [
                  "fe69ea4d.151248"
              ],
              [
                  "4f4ae634.f8af28"
              ]
          ]
      },
      {
          "id": "727a11e6.dd5e3",
          "type": "geofence",
          "z": "6ac85c37.86cd94",
          "name": "京津冀",
          "manager": "856eb3df.cdbeb",
          "centre": null,
          "radius": null,
          "points": [],
          "geofenceName": null,
          "fenceID": null,
          "mode": null,
          "x": 670,
          "y": 400,
          "wires": [
              [
                  "44ee0072.f5d5"
              ]
          ]
      },
      {
          "id": "ee73dd7e.b6238",
          "type": "function",
          "z": "6ac85c37.86cd94",
          "name": "Add person to map",
          "func": "var line=flow.get('line') || [];\nline.push([msg.payload.lat,msg.payload.lon]);\nif(line.length >1000){\n    line.pop();\n}\nflow.set('line',line);\n\nvar thing = {\n    name:\"Jason Isaacs\", \n    lat:msg.payload.lat, \n    lon:msg.payload.lon,\n    icon:msg.payload.icon,\n    iconColor:msg.payload.iconColor,\n    extrainfo:\"Hello to Jason Isaacs\",\n    line:line\n};\nmsg.payload = thing;\nreturn msg;",
          "outputs": 1,
          "noerr": 0,
          "x": 510,
          "y": 360,
          "wires": [
              [
                  "c16fb04d.9f481"
              ]
          ]
      },
      {
          "id": "d2cf5607.08bdb8",
          "type": "change",
          "z": "6ac85c37.86cd94",
          "name": "",
          "rules": [
              {
                  "t": "set",
                  "p": "payload.lat",
                  "pt": "msg",
                  "to": "payload.latitude",
                  "tot": "msg"
              },
              {
                  "t": "set",
                  "p": "payload.lon",
                  "pt": "msg",
                  "to": "payload.longitude",
                  "tot": "msg"
              },
              {
                  "t": "set",
                  "p": "payload.icon",
                  "pt": "msg",
                  "to": "ENOPA-------",
                  "tot": "str"
              },
              {
                  "t": "set",
                  "p": "payload.iconColor",
                  "pt": "msg",
                  "to": "darkred",
                  "tot": "str"
              }
          ],
          "action": "",
          "property": "",
          "from": "",
          "to": "",
          "reg": false,
          "x": 480,
          "y": 480,
          "wires": [
              [
                  "727a11e6.dd5e3",
                  "ee73dd7e.b6238"
              ]
          ]
      },
      {
          "id": "44ee0072.f5d5",
          "type": "rbe",
          "z": "6ac85c37.86cd94",
          "name": "",
          "func": "rbe",
          "gap": "",
          "start": "",
          "inout": "out",
          "property": "payload.in",
          "x": 790,
          "y": 340,
          "wires": [
              [
                  "fd12f720.e0e6c8"
              ]
          ]
      },
      {
          "id": "fd12f720.e0e6c8",
          "type": "function",
          "z": "6ac85c37.86cd94",
          "name": "",
          "func": "var msg2={}\nmsg2.payload = msg.payload.in? \"欢迎你来到\"+msg.payload.name:msg.payload.name+\",欢迎您再来！\"\nreturn msg2;",
          "outputs": 1,
          "noerr": 0,
          "x": 910,
          "y": 280,
          "wires": [
              [
                  "16ced3d5.5de30c"
              ]
          ]
      },
      {
          "id": "c78b7342.5345d",
          "type": "tcp in",
          "z": "6ac85c37.86cd94",
          "name": "",
          "server": "client",
          "host": "192.168.200.24",
          "port": "2222",
          "datamode": "stream",
          "datatype": "utf8",
          "newline": "",
          "topic": "",
          "base64": false,
          "x": 290,
          "y": 100,
          "wires": [
              [
                  "7d768b72.4f46e4"
              ]
          ]
      },
      {
          "id": "61b21924.e35428",
          "type": "comment",
          "z": "6ac85c37.86cd94",
          "name": "使用gpsfeed+和com0com或者NMEA_Simulator来注入数据",
          "info": "",
          "x": 330,
          "y": 300,
          "wires": []
      },
      {
          "id": "934d9519.637ab8",
          "type": "comment",
          "z": "6ac85c37.86cd94",
          "name": "手动注入，在地图上绘制多边形",
          "info": "",
          "x": 230,
          "y": 160,
          "wires": []
      },
      {
          "id": "9aa6ff84.766b1",
          "type": "comment",
          "z": "6ac85c37.86cd94",
          "name": "使用gpsfeed+ 生成数据，用telnet  127.0.0.1  2222看数据",
          "info": "",
          "x": 290,
          "y": 60,
          "wires": []
      },
      {
          "id": "da4ebba1.a18ed8",
          "type": "udp in",
          "z": "6ac85c37.86cd94",
          "name": "",
          "iface": "",
          "port": "1365",
          "ipv": "udp4",
          "multicast": "false",
          "group": "",
          "datatype": "utf8",
          "x": 450,
          "y": 140,
          "wires": [
              [
                  "7d768b72.4f46e4"
              ]
          ]
      },
      {
          "id": "16ced3d5.5de30c",
          "type": "xmpp out",
          "z": "6ac85c37.86cd94",
          "name": "物联网设备群",
          "server": "11e1164e.4cc4ba",
          "to": "8000490@groupchat.xtpt.e-u.cn",
          "join": false,
          "sendObject": false,
          "x": 980,
          "y": 380,
          "wires": []
      },
      {
          "id": "9e047bd7.236988",
          "type": "inject",
          "z": "6ac85c37.86cd94",
          "name": "",
          "topic": "",
          "payload": "",
          "payloadType": "date",
          "repeat": "",
          "crontab": "",
          "once": false,
          "onceDelay": 0.1,
          "x": 170,
          "y": 1320,
          "wires": [
              [
                  "27bb7d7d.15edc2"
              ]
          ]
      },
      {
          "id": "27bb7d7d.15edc2",
          "type": "debug",
          "z": "6ac85c37.86cd94",
          "name": "",
          "active": true,
          "tosidebar": true,
          "console": false,
          "tostatus": false,
          "complete": "false",
          "x": 510,
          "y": 1320,
          "wires": []
      },
      {
          "id": "1f5b146e.609e6c",
          "type": "mqtt in",
          "z": "6ac85c37.86cd94",
          "name": "192.168.15.100 sensor#",
          "topic": "sensor/#",
          "qos": "2",
          "broker": "f4ba2eeb.5f66a",
          "x": 390,
          "y": 1440,
          "wires": [
              [
                  "9a69f439.e7c868"
              ]
          ]
      },
      {
          "id": "aa2eed3f.39b3a",
          "type": "mqtt out",
          "z": "6ac85c37.86cd94",
          "name": "192.168.15.100 sensor/demo",
          "topic": "sensor/demo",
          "qos": "",
          "retain": "",
          "broker": "f4ba2eeb.5f66a",
          "x": 410,
          "y": 1380,
          "wires": []
      },
      {
          "id": "b6759dcb.adf8f",
          "type": "inject",
          "z": "6ac85c37.86cd94",
          "name": "",
          "topic": "",
          "payload": "",
          "payloadType": "date",
          "repeat": "",
          "crontab": "",
          "once": false,
          "onceDelay": 0.1,
          "x": 180,
          "y": 1380,
          "wires": [
              [
                  "aa2eed3f.39b3a"
              ]
          ]
      },
      {
          "id": "9a69f439.e7c868",
          "type": "debug",
          "z": "6ac85c37.86cd94",
          "name": "",
          "active": true,
          "tosidebar": true,
          "console": false,
          "tostatus": false,
          "complete": "false",
          "x": 550,
          "y": 1440,
          "wires": []
      },
      {
          "id": "94ab8b37.808488",
          "type": "mqtt in",
          "z": "6ac85c37.86cd94",
          "name": "iot.eclipse  sensor/#",
          "topic": "sensor/#",
          "qos": "2",
          "broker": "7080338a.3aa73c",
          "x": 370,
          "y": 1600,
          "wires": [
              [
                  "3c0dfdb3.9d8992"
              ]
          ]
      },
      {
          "id": "417946bb.3054f8",
          "type": "mqtt out",
          "z": "6ac85c37.86cd94",
          "name": "iot.eclipse.org sensor/demo",
          "topic": "sensor/demo",
          "qos": "",
          "retain": "",
          "broker": "7080338a.3aa73c",
          "x": 400,
          "y": 1540,
          "wires": []
      },
      {
          "id": "b2d329fc.030b78",
          "type": "inject",
          "z": "6ac85c37.86cd94",
          "name": "",
          "topic": "",
          "payload": "",
          "payloadType": "date",
          "repeat": "",
          "crontab": "",
          "once": false,
          "onceDelay": 0.1,
          "x": 180,
          "y": 1540,
          "wires": [
              [
                  "417946bb.3054f8"
              ]
          ]
      },
      {
          "id": "3c0dfdb3.9d8992",
          "type": "debug",
          "z": "6ac85c37.86cd94",
          "name": "",
          "active": true,
          "tosidebar": true,
          "console": false,
          "tostatus": false,
          "complete": "false",
          "x": 650,
          "y": 1580,
          "wires": []
      },
      {
          "id": "a70cd2b3.3fa06",
          "type": "serial-port",
          "z": "",
          "serialport": "COM9",
          "serialbaud": "4800",
          "databits": "8",
          "parity": "none",
          "stopbits": "1",
          "newline": "\\n",
          "bin": "false",
          "out": "char",
          "addchar": false
      },
      {
          "id": "856eb3df.cdbeb",
          "type": "geofence-manager",
          "z": "",
          "name": "Geofence manager2",
          "geofences": {
              "727a11e6.dd5e3": {
                  "type": "Feature",
                  "properties": {},
                  "geometry": {
                      "type": "Polygon",
                      "coordinates": [
                          [
                              [
                                  116.312256,
                                  38.496594
                              ],
                              [
                                  116.729736,
                                  39.181175
                              ],
                              [
                                  117.597656,
                                  39.061849
                              ],
                              [
                                  117.531738,
                                  38.444985
                              ],
                              [
                                  116.773682,
                                  38.177751
                              ],
                              [
                                  116.312256,
                                  38.496594
                              ]
                          ]
                      ]
                  }
              }
          }
      },
      {
          "id": "11e1164e.4cc4ba",
          "type": "xmpp-server",
          "z": "",
          "server": "xtpt.e-u.cn",
          "port": "5222",
          "nickname": "wangf"
      },
      {
          "id": "f4ba2eeb.5f66a",
          "type": "mqtt-broker",
          "z": "",
          "name": "",
          "broker": "192.168.15.100",
          "port": "1883",
          "clientid": "",
          "usetls": false,
          "compatmode": true,
          "keepalive": "60",
          "cleansession": true,
          "birthTopic": "",
          "birthQos": "0",
          "birthPayload": "",
          "closeTopic": "",
          "closeQos": "0",
          "closePayload": "",
          "willTopic": "",
          "willQos": "0",
          "willPayload": ""
      },
      {
          "id": "7080338a.3aa73c",
          "type": "mqtt-broker",
          "z": "",
          "name": "",
          "broker": "iot.eclipse.org",
          "port": "1883",
          "clientid": "",
          "usetls": false,
          "compatmode": true,
          "keepalive": "60",
          "cleansession": true,
          "birthTopic": "",
          "birthQos": "0",
          "birthPayload": "",
          "closeTopic": "",
          "closeQos": "0",
          "closePayload": "",
          "willTopic": "",
          "willQos": "0",
          "willPayload": ""
      }
  ]
  ```

  

# 3、定制 node-red

> 用文本编辑器打开C:\Users\wangf\.node-red\settings.js文件，修改默认配置，可以定制node-red。
>
> 我只改了ip地址、添加了一个nodejs的gps解析包，修改了主题，启用了项目管理。其它配置项待以后探索。

```javascript
/**
 * Copyright JS Foundation and other contributors, http://js.foundation
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 **/

// The `https` setting requires the `fs` module. Uncomment the following
// to make it available:
//var fs = require("fs");

module.exports = {
    // the tcp port that the Node-RED web server is listening on
    uiPort: process.env.PORT || 1880,

    // By default, the Node-RED UI accepts connections on all IPv4 interfaces.
    // To listen on all IPv6 addresses, set uiHost to "::",
    // The following property can be used to listen on a specific interface. For
    // example, the following would only allow connections from the local machine.
    //uiHost: "127.0.0.1",
    //修改IP地址
    uiHost: "192.168.200.24",
    // Retry time in milliseconds for MQTT connections
    mqttReconnectTime: 15000,

    // Retry time in milliseconds for Serial port connections
    serialReconnectTime: 15000,

    // Retry time in milliseconds for TCP socket connections
    //socketReconnectTime: 10000,

    // Timeout in milliseconds for TCP server socket connections
    //  defaults to no timeout
    //socketTimeout: 120000,

    // Timeout in milliseconds for HTTP request connections
    //  defaults to 120 seconds
    //httpRequestTimeout: 120000,

    // The maximum length, in characters, of any message sent to the debug sidebar tab
    debugMaxLength: 1000,

    // The maximum number of messages nodes will buffer internally as part of their
    // operation. This applies across a range of nodes that operate on message sequences.
    //  defaults to no limit. A value of 0 also means no limit is applied.
    //nodeMaxMessageBufferLength: 0,

    // To disable the option for using local files for storing keys and certificates in the TLS configuration
    //  node, set this to true
    //tlsConfigDisableLocalFiles: true,

    // Colourise the console output of the debug node
    //debugUseColors: true,

    // The file containing the flows. If not set, it defaults to flows_<hostname>.json
    //flowFile: 'flows.json',

    // To enabled pretty-printing of the flow within the flow file, set the following
    //  property to true:
    //flowFilePretty: true,

    // By default, credentials are encrypted in storage using a generated key. To
    // specify your own secret, set the following property.
    // If you want to disable encryption of credentials, set this property to false.
    // Note: once you set this property, do not change it - doing so will prevent
    // node-red from being able to decrypt your existing credentials and they will be
    // lost.
    //credentialSecret: "a-secret-key",

    // By default, all user data is stored in the Node-RED install directory. To
    // use a different location, the following property can be used
    //userDir: '/home/nol/.node-red/',

    // Node-RED scans the `nodes` directory in the install directory to find nodes.
    // The following property can be used to specify an additional directory to scan.
    //nodesDir: '/home/nol/.node-red/nodes',

    // By default, the Node-RED UI is available at http://localhost:1880/
    // The following property can be used to specify a different root path.
    // If set to false, this is disabled.
    //httpAdminRoot: '/admin',

    // Some nodes, such as HTTP In, can be used to listen for incoming http requests.
    // By default, these are served relative to '/'. The following property
    // can be used to specifiy a different root path. If set to false, this is
    // disabled.
    //httpNodeRoot: '/red-nodes',

    // The following property can be used in place of 'httpAdminRoot' and 'httpNodeRoot',
    // to apply the same root to both parts.
    //httpRoot: '/red',

    // When httpAdminRoot is used to move the UI to a different root path, the
    // following property can be used to identify a directory of static content
    // that should be served at http://localhost:1880/.
    //httpStatic: '/home/nol/node-red-static/',

    // The maximum size of HTTP request that will be accepted by the runtime api.
    // Default: 5mb
    //apiMaxLength: '5mb',

    // If you installed the optional node-red-dashboard you can set it's path
    // relative to httpRoot
    //ui: { path: "ui" },

    // Securing Node-RED
    // -----------------
    // To password protect the Node-RED editor and admin API, the following
    // property can be used. See http://nodered.org/docs/security.html for details.
    //adminAuth: {
    //    type: "credentials",
    //    users: [{
    //        username: "admin",
    //        password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
    //        permissions: "*"
    //    }]
    //},

    // To password protect the node-defined HTTP endpoints (httpNodeRoot), or
    // the static content (httpStatic), the following properties can be used.
    // The pass field is a bcrypt hash of the password.
    // See http://nodered.org/docs/security.html#generating-the-password-hash
    //httpNodeAuth: {user:"user",pass:"$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN."},
    //httpStaticAuth: {user:"user",pass:"$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN."},

    // The following property can be used to enable HTTPS
    // See http://nodejs.org/api/https.html#https_https_createserver_options_requestlistener
    // for details on its contents.
    // See the comment at the top of this file on how to load the `fs` module used by
    // this setting.
    //
    //https: {
    //    key: fs.readFileSync('privatekey.pem'),
    //    cert: fs.readFileSync('certificate.pem')
    //},

    // The following property can be used to cause insecure HTTP connections to
    // be redirected to HTTPS.
    //requireHttps: true

    // The following property can be used to disable the editor. The admin API
    // is not affected by this option. To disable both the editor and the admin
    // API, use either the httpRoot or httpAdminRoot properties
    //disableEditor: false,

    // The following property can be used to configure cross-origin resource sharing
    // in the HTTP nodes.
    // See https://github.com/troygoode/node-cors#configuration-options for
    // details on its contents. The following is a basic permissive set of options:
    //httpNodeCors: {
    //    origin: "*",
    //    methods: "GET,PUT,POST,DELETE"
    //},

    // If you need to set an http proxy please set an environment variable
    // called http_proxy (or HTTP_PROXY) outside of Node-RED in the operating system.
    // For example - http_proxy=http://myproxy.com:8080
    // (Setting it here will have no effect)
    // You may also specify no_proxy (or NO_PROXY) to supply a comma separated
    // list of domains to not proxy, eg - no_proxy=.acme.co,.acme.co.uk

    // The following property can be used to add a custom middleware function
    // in front of all http in nodes. This allows custom authentication to be
    // applied to all http in nodes, or any other sort of common request processing.
    //httpNodeMiddleware: function(req,res,next) {
    //    // Handle/reject the request, or pass it on to the http in node by calling next();
    //    // Optionally skip our rawBodyParser by setting this to true;
    //    //req.skipRawBodyParser = true;
    //    next();
    //},

    // The following property can be used to verify websocket connection attempts.
    // This allows, for example, the HTTP request headers to be checked to ensure
    // they include valid authentication information.
    //webSocketNodeVerifyClient: function(info) {
    //    // 'info' has three properties:
    //    //   - origin : the value in the Origin header
    //    //   - req : the HTTP request
    //    //   - secure : true if req.connection.authorized or req.connection.encrypted is set
    //    //
    //    // The function should return true if the connection should be accepted, false otherwise.
    //    //
    //    // Alternatively, if this function is defined to accept a second argument, callback,
    //    // it can be used to verify the client asynchronously.
    //    // The callback takes three arguments:
    //    //   - result : boolean, whether to accept the connection or not
    //    //   - code : if result is false, the HTTP error status to return
    //    //   - reason: if result is false, the HTTP reason string to return
    //},

    // Anything in this hash is globally available to all functions.
    // It is accessed as context.global.
    // eg:
    //    functionGlobalContext: { os:require('os') }
    // can be accessed in a function block as:
    //    context.global.os

    functionGlobalContext: {
         //osss:require('os'),
        // jfive:require("johnny-five"),
        // j5board:require("johnny-five").Board({repl:false})
        //nmea:require('node-nmea'),
        //加载第三方的nodejs包，首先要用npm 安装，如：npm install nmea-simple
        //nmea-simple这个包是一个解析gps信号的nodejs包。
        nmea: require('nmea-simple'),
        
    },

    // The following property can be used to order the categories in the editor
    // palette. If a node's category is not in the list, the category will get
    // added to the end of the palette.
    // If not set, the following default order is used:
    //paletteCategories: ['subflows', 'input', 'output', 'function', 'social', 'mobile', 'storage', 'analysis', 'advanced'],

    // Configure the logging output
    logging: {
        // Only console logging is currently supported
        console: {
            // Level of logging to be recorded. Options are:
            // fatal - only those errors which make the application unusable should be recorded
            // error - record errors which are deemed fatal for a particular request + fatal errors
            // warn - record problems which are non fatal + errors + fatal errors
            // info - record information about the general running of the application + warn + error + fatal errors
            // debug - record information which is more verbose than info + info + warn + error + fatal errors
            // trace - record very detailed logging + debug + info + warn + error + fatal errors
            // off - turn off all logging (doesn't affect metrics or audit)
            level: "info",
            // Whether or not to include metric events in the log output
            metrics: false,
            // Whether or not to include audit events in the log output
            audit: false
        }
    },

    // Customising the editor
    editorTheme: {
        page: {
            title: "天融IoT可视化编程与边缘计算平台",
            //favicon: "/absolute/path/to/theme/icon",
            //css: "/absolute/path/to/custom/css/file"
        },
        header: {
            title: "天融IoT可视化编程与边缘计算平台",
            //image: "/absolute/path/to/header/image", // or null to remove image
            url: "http://www.e-u.cn" // optional url to make the header text/image a link to this url
        },        
        projects: {
            // To enable the Projects feature, set this value to true
            //支持项目管理
            enabled: true
        }
    }
}
```



# 4、进阶

## 4.1、地理围栏例子

> 为了进一步体验node-red的功能，请导入下面的流，进行进阶体验。
>
> 该流中用到了串口、地图、地理围栏、switch、node-nmea包、function、change、xmpp等节点，实现了gps信号（使用gpsfeed+模拟器）的处理。
>
> > 注意：该流中使用的一些节点默认安装版中是不存在的，导入后，你可以依据提示，从右上角菜单中选择【节点管理】查找并安装节点。

```javascript
[
    {
        "id": "c16fb04d.9f481",
        "type": "worldmap",
        "z": "6ac85c37.86cd94",
        "name": "",
        "lat": "38",
        "lon": "120",
        "zoom": "",
        "layer": "OSM",
        "cluster": "",
        "maxage": "",
        "usermenu": "show",
        "layers": "show",
        "panit": "true",
        "x": 770,
        "y": 220,
        "wires": []
    },
    {
        "id": "c32606fa.45cb38",
        "type": "inject",
        "z": "6ac85c37.86cd94",
        "name": "",
        "topic": "",
        "payload": "",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "",
        "x": 250,
        "y": 200,
        "wires": [
            [
                "a9e482ae.e2e9"
            ]
        ]
    },
    {
        "id": "a9e482ae.e2e9",
        "type": "function",
        "z": "6ac85c37.86cd94",
        "name": "Add person to map",
        "func": "var geo = { \"type\": \"FeatureCollection\",\n\"features\": [\n  {\n    \"type\": \"Feature\",\n    \"properties\": {\n      \"color\": \"rgb(0,0,255)\",\n      \"roofColor\": \"rgb(128,128,255)\",\n      \"height\": 20,\n      \"minHeight\": 0\n    },\n    \"geometry\": {\n      \"type\": \"Polygon\",\n      \"coordinates\": [\n        [\n        [117.356221,37.048611],\n        [119.356039,38 + Math.random()],\n        [119.355765,39 + Math.random()],\n        [119.355937,39 + Math.random()],\n        [117.356221,37.048611]\n        ]\n      ]\n    }\n  }\n]\n}\nvar m = {overlay:\"Golf Clubhouse\", geojson:geo, fit:true};\nmsg.payload = {command:{map:m, lat:37.0484, lon:117.3558}};\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 490,
        "y": 200,
        "wires": [
            [
                "c16fb04d.9f481"
            ]
        ]
    },
    {
        "id": "cf043790.be4598",
        "type": "comment",
        "z": "6ac85c37.86cd94",
        "name": "http://(your-server-ip):1880/worldmap  或者 ctrl+shift+md打开地图",
        "info": "Adds a map at http://(your-server-ip):1880/worldmap. \n\nThe `function` node creates an object with some basic properties required to add to a map.",
        "x": 570,
        "y": 20,
        "wires": []
    },
    {
        "id": "ce262b25.8fb818",
        "type": "serial in",
        "z": "6ac85c37.86cd94",
        "name": "",
        "serial": "a70cd2b3.3fa06",
        "x": 150,
        "y": 440,
        "wires": [
            [
                "bd386d41.de885"
            ]
        ]
    },
    {
        "id": "7d768b72.4f46e4",
        "type": "debug",
        "z": "6ac85c37.86cd94",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "x": 730,
        "y": 100,
        "wires": []
    },
    {
        "id": "fe69ea4d.151248",
        "type": "function",
        "z": "6ac85c37.86cd94",
        "name": "",
        "func": "var nmea=global.get(\"nmea\");\nvar data = nmea.parseNmeaSentence(msg.payload);\nmsg.payload=data\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 290,
        "y": 380,
        "wires": [
            [
                "d2cf5607.08bdb8"
            ]
        ]
    },
    {
        "id": "91a85599.1b9728",
        "type": "debug",
        "z": "6ac85c37.86cd94",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "x": 750,
        "y": 580,
        "wires": []
    },
    {
        "id": "4f4ae634.f8af28",
        "type": "change",
        "z": "6ac85c37.86cd94",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "WIND",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 450,
        "y": 600,
        "wires": [
            [
                "91a85599.1b9728"
            ]
        ]
    },
    {
        "id": "bd386d41.de885",
        "type": "switch",
        "z": "6ac85c37.86cd94",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "cont",
                "v": "$GPRMC",
                "vt": "str"
            },
            {
                "t": "cont",
                "v": "$WIMWD",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 2,
        "x": 290,
        "y": 540,
        "wires": [
            [
                "fe69ea4d.151248"
            ],
            [
                "4f4ae634.f8af28"
            ]
        ]
    },
    {
        "id": "727a11e6.dd5e3",
        "type": "geofence",
        "z": "6ac85c37.86cd94",
        "name": "京津冀",
        "manager": "856eb3df.cdbeb",
        "centre": null,
        "radius": null,
        "points": [],
        "geofenceName": null,
        "fenceID": null,
        "mode": null,
        "x": 670,
        "y": 400,
        "wires": [
            [
                "44ee0072.f5d5"
            ]
        ]
    },
    {
        "id": "ee73dd7e.b6238",
        "type": "function",
        "z": "6ac85c37.86cd94",
        "name": "Add person to map",
        "func": "var line=flow.get('line') || [];\nline.push([msg.payload.lat,msg.payload.lon]);\nif(line.length >1000){\n    line.pop();\n}\nflow.set('line',line);\n\nvar thing = {\n    name:\"Jason Isaacs\", \n    lat:msg.payload.lat, \n    lon:msg.payload.lon,\n    icon:msg.payload.icon,\n    iconColor:msg.payload.iconColor,\n    extrainfo:\"Hello to Jason Isaacs\",\n    line:line\n};\nmsg.payload = thing;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 510,
        "y": 360,
        "wires": [
            [
                "c16fb04d.9f481"
            ]
        ]
    },
    {
        "id": "d2cf5607.08bdb8",
        "type": "change",
        "z": "6ac85c37.86cd94",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "payload.lat",
                "pt": "msg",
                "to": "payload.latitude",
                "tot": "msg"
            },
            {
                "t": "set",
                "p": "payload.lon",
                "pt": "msg",
                "to": "payload.longitude",
                "tot": "msg"
            },
            {
                "t": "set",
                "p": "payload.icon",
                "pt": "msg",
                "to": "ENOPA-------",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "payload.iconColor",
                "pt": "msg",
                "to": "darkred",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 480,
        "y": 480,
        "wires": [
            [
                "727a11e6.dd5e3",
                "ee73dd7e.b6238"
            ]
        ]
    },
    {
        "id": "44ee0072.f5d5",
        "type": "rbe",
        "z": "6ac85c37.86cd94",
        "name": "",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "property": "payload.in",
        "x": 790,
        "y": 340,
        "wires": [
            [
                "fd12f720.e0e6c8"
            ]
        ]
    },
    {
        "id": "fd12f720.e0e6c8",
        "type": "function",
        "z": "6ac85c37.86cd94",
        "name": "",
        "func": "var msg2={}\nmsg2.payload = msg.payload.in? \"欢迎你来到\"+msg.payload.name:msg.payload.name+\",欢迎您再来！\"\nreturn msg2;",
        "outputs": 1,
        "noerr": 0,
        "x": 910,
        "y": 280,
        "wires": [
            [
                "16ced3d5.5de30c"
            ]
        ]
    },
    {
        "id": "c78b7342.5345d",
        "type": "tcp in",
        "z": "6ac85c37.86cd94",
        "name": "",
        "server": "client",
        "host": "192.168.200.24",
        "port": "2222",
        "datamode": "stream",
        "datatype": "utf8",
        "newline": "",
        "topic": "",
        "base64": false,
        "x": 290,
        "y": 100,
        "wires": [
            [
                "7d768b72.4f46e4"
            ]
        ]
    },
    {
        "id": "61b21924.e35428",
        "type": "comment",
        "z": "6ac85c37.86cd94",
        "name": "使用gpsfeed+和com0com或者NMEA_Simulator来注入数据",
        "info": "",
        "x": 330,
        "y": 300,
        "wires": []
    },
    {
        "id": "934d9519.637ab8",
        "type": "comment",
        "z": "6ac85c37.86cd94",
        "name": "手动注入，在地图上绘制多边形",
        "info": "",
        "x": 230,
        "y": 160,
        "wires": []
    },
    {
        "id": "9aa6ff84.766b1",
        "type": "comment",
        "z": "6ac85c37.86cd94",
        "name": "使用gpsfeed+ 生成数据，用telnet  127.0.0.1  2222看数据",
        "info": "",
        "x": 290,
        "y": 60,
        "wires": []
    },
    {
        "id": "da4ebba1.a18ed8",
        "type": "udp in",
        "z": "6ac85c37.86cd94",
        "name": "",
        "iface": "",
        "port": "1365",
        "ipv": "udp4",
        "multicast": "false",
        "group": "",
        "datatype": "utf8",
        "x": 450,
        "y": 140,
        "wires": [
            [
                "7d768b72.4f46e4"
            ]
        ]
    },
    {
        "id": "16ced3d5.5de30c",
        "type": "xmpp out",
        "z": "6ac85c37.86cd94",
        "name": "物联网设备群",
        "server": "11e1164e.4cc4ba",
        "to": "8000490@groupchat.xtpt.e-u.cn",
        "join": false,
        "sendObject": false,
        "x": 980,
        "y": 380,
        "wires": []
    },
    {
        "id": "9e047bd7.236988",
        "type": "inject",
        "z": "6ac85c37.86cd94",
        "name": "",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "x": 170,
        "y": 1320,
        "wires": [
            [
                "27bb7d7d.15edc2"
            ]
        ]
    },
    {
        "id": "27bb7d7d.15edc2",
        "type": "debug",
        "z": "6ac85c37.86cd94",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "x": 650,
        "y": 1320,
        "wires": []
    },
    {
        "id": "1f5b146e.609e6c",
        "type": "mqtt in",
        "z": "6ac85c37.86cd94",
        "name": "192.168.15.100 sensor#",
        "topic": "sensor/#",
        "qos": "2",
        "broker": "f4ba2eeb.5f66a",
        "x": 390,
        "y": 1440,
        "wires": [
            [
                "9a69f439.e7c868"
            ]
        ]
    },
    {
        "id": "aa2eed3f.39b3a",
        "type": "mqtt out",
        "z": "6ac85c37.86cd94",
        "name": "192.168.15.100 sensor/demo",
        "topic": "sensor/demo",
        "qos": "",
        "retain": "",
        "broker": "f4ba2eeb.5f66a",
        "x": 410,
        "y": 1380,
        "wires": []
    },
    {
        "id": "b6759dcb.adf8f",
        "type": "inject",
        "z": "6ac85c37.86cd94",
        "name": "",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "x": 180,
        "y": 1380,
        "wires": [
            [
                "aa2eed3f.39b3a"
            ]
        ]
    },
    {
        "id": "9a69f439.e7c868",
        "type": "debug",
        "z": "6ac85c37.86cd94",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "x": 690,
        "y": 1440,
        "wires": []
    },
    {
        "id": "94ab8b37.808488",
        "type": "mqtt in",
        "z": "6ac85c37.86cd94",
        "name": "iot.eclipse  sensor/#",
        "topic": "sensor/#",
        "qos": "2",
        "broker": "7080338a.3aa73c",
        "x": 370,
        "y": 1600,
        "wires": [
            [
                "3c0dfdb3.9d8992"
            ]
        ]
    },
    {
        "id": "417946bb.3054f8",
        "type": "mqtt out",
        "z": "6ac85c37.86cd94",
        "name": "iot.eclipse.org sensor/demo",
        "topic": "sensor/demo",
        "qos": "",
        "retain": "",
        "broker": "7080338a.3aa73c",
        "x": 400,
        "y": 1540,
        "wires": []
    },
    {
        "id": "b2d329fc.030b78",
        "type": "inject",
        "z": "6ac85c37.86cd94",
        "name": "",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "x": 180,
        "y": 1540,
        "wires": [
            [
                "417946bb.3054f8"
            ]
        ]
    },
    {
        "id": "3c0dfdb3.9d8992",
        "type": "debug",
        "z": "6ac85c37.86cd94",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "x": 650,
        "y": 1600,
        "wires": []
    },
    {
        "id": "a70cd2b3.3fa06",
        "type": "serial-port",
        "z": "",
        "serialport": "COM9",
        "serialbaud": "4800",
        "databits": "8",
        "parity": "none",
        "stopbits": "1",
        "newline": "\\n",
        "bin": "false",
        "out": "char",
        "addchar": false
    },
    {
        "id": "856eb3df.cdbeb",
        "type": "geofence-manager",
        "z": "",
        "name": "Geofence manager2",
        "geofences": {
            "727a11e6.dd5e3": {
                "type": "Feature",
                "properties": {},
                "geometry": {
                    "type": "Polygon",
                    "coordinates": [
                        [
                            [
                                116.312256,
                                38.496594
                            ],
                            [
                                116.729736,
                                39.181175
                            ],
                            [
                                117.597656,
                                39.061849
                            ],
                            [
                                117.531738,
                                38.444985
                            ],
                            [
                                116.773682,
                                38.177751
                            ],
                            [
                                116.312256,
                                38.496594
                            ]
                        ]
                    ]
                }
            }
        }
    },
    {
        "id": "11e1164e.4cc4ba",
        "type": "xmpp-server",
        "z": "",
        "server": "xtpt.e-u.cn",
        "port": "5222",
        "nickname": "wangf"
    },
    {
        "id": "f4ba2eeb.5f66a",
        "type": "mqtt-broker",
        "z": "",
        "name": "",
        "broker": "192.168.15.100",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willPayload": ""
    },
    {
        "id": "7080338a.3aa73c",
        "type": "mqtt-broker",
        "z": "",
        "name": "",
        "broker": "iot.eclipse.org",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willPayload": ""
    }
]
```

## 4.2、简单的Dashboard

```javascript
[
    {
        "id": "51722997.b1a6b8",
        "type": "mqtt in",
        "z": "3d6f79af.fcbd16",
        "name": "",
        "topic": "#",
        "qos": "2",
        "broker": "948d0e78.d504d",
        "x": 250,
        "y": 260,
        "wires": [
            [
                "bba8beb5.9777a"
            ]
        ]
    },
    {
        "id": "bffbdef4.ac064",
        "type": "ui_gauge",
        "z": "3d6f79af.fcbd16",
        "name": "",
        "group": "dcc211a1.e5597",
        "order": 8,
        "width": "15",
        "height": "4",
        "gtype": "gage",
        "title": "游客数量",
        "label": "万人",
        "format": "{{msg.payload.counter}}",
        "min": "-100",
        "max": "100",
        "colors": [
            "#00b500",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "-80",
        "seg2": "-60",
        "x": 720,
        "y": 480,
        "wires": []
    },
    {
        "id": "20ecd59a.09f09a",
        "type": "ui_chart",
        "z": "3d6f79af.fcbd16",
        "name": "",
        "group": "2a127dbd.3ef0f2",
        "order": 7,
        "width": "0",
        "height": "0",
        "label": "熵情",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "bezier",
        "nodata": "",
        "dot": false,
        "ymin": "0",
        "ymax": "100",
        "removeOlder": "2",
        "removeOlderPoints": "100",
        "removeOlderUnit": "60",
        "cutout": 0,
        "useOneColor": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "useOldStyle": false,
        "x": 770,
        "y": 300,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "bba8beb5.9777a",
        "type": "function",
        "z": "3d6f79af.fcbd16",
        "name": "str2json",
        "func": "msg.payload=JSON.parse(msg.payload);\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 400,
        "y": 300,
        "wires": [
            [
                "3b2a01bc.37fd5e",
                "bffbdef4.ac064",
                "65ef5833.a1d708",
                "3f452126.fc60de"
            ]
        ]
    },
    {
        "id": "3b2a01bc.37fd5e",
        "type": "function",
        "z": "3d6f79af.fcbd16",
        "name": "getCounter",
        "func": "msg.payload=msg.payload.counter\nreturn msg ;",
        "outputs": 1,
        "noerr": 0,
        "x": 550,
        "y": 260,
        "wires": [
            [
                "20ecd59a.09f09a"
            ]
        ]
    },
    {
        "id": "65ef5833.a1d708",
        "type": "function",
        "z": "3d6f79af.fcbd16",
        "name": "getCounter2",
        "func": "msg.payload=msg.payload.counter - Math.floor(Math.random()*10+1)\nreturn msg ;",
        "outputs": 1,
        "noerr": 0,
        "x": 550,
        "y": 180,
        "wires": [
            [
                "c0b728f.c3caad8",
                "18ff46cd.fb1d99"
            ]
        ]
    },
    {
        "id": "c0b728f.c3caad8",
        "type": "ui_chart",
        "z": "3d6f79af.fcbd16",
        "name": "",
        "group": "2a127dbd.3ef0f2",
        "order": 0,
        "width": "0",
        "height": "0",
        "label": "湿度",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "step",
        "nodata": "",
        "dot": false,
        "ymin": "0",
        "ymax": "100",
        "removeOlder": 1,
        "removeOlderPoints": "",
        "removeOlderUnit": "3600",
        "cutout": 0,
        "useOneColor": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "useOldStyle": false,
        "x": 780,
        "y": 200,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "18ff46cd.fb1d99",
        "type": "ui_text",
        "z": "3d6f79af.fcbd16",
        "group": "7237bf58.e708f",
        "order": 0,
        "width": 0,
        "height": 0,
        "name": "",
        "label": "text",
        "format": "{{msg.payload}}",
        "layout": "row-spread",
        "x": 780,
        "y": 140,
        "wires": []
    },
    {
        "id": "3f452126.fc60de",
        "type": "function",
        "z": "3d6f79af.fcbd16",
        "name": "getCounter2",
        "func": "msg.payload=msg.payload.counter - Math.floor(Math.random()*10+1)\nreturn msg ;",
        "outputs": 1,
        "noerr": 0,
        "x": 550,
        "y": 120,
        "wires": [
            [
                "5775e29e.4a0a7c"
            ]
        ]
    },
    {
        "id": "5775e29e.4a0a7c",
        "type": "ui_chart",
        "z": "3d6f79af.fcbd16",
        "name": "",
        "group": "dcc211a1.e5597",
        "order": 0,
        "width": "0",
        "height": "0",
        "label": "最近一小时",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "linear",
        "nodata": "",
        "dot": false,
        "ymin": "0",
        "ymax": "100",
        "removeOlder": 1,
        "removeOlderPoints": "",
        "removeOlderUnit": "3600",
        "cutout": 0,
        "useOneColor": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "useOldStyle": false,
        "x": 790,
        "y": 80,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "948d0e78.d504d",
        "type": "mqtt-broker",
        "z": "",
        "name": "",
        "broker": "192.168.15.100",
        "port": "1883",
        "clientid": "node-002",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "closeTopic": "",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willPayload": ""
    },
    {
        "id": "dcc211a1.e5597",
        "type": "ui_group",
        "z": "",
        "name": "旅游",
        "tab": "ea08059f.131878",
        "order": 2,
        "disp": true,
        "width": "15",
        "collapse": true
    },
    {
        "id": "2a127dbd.3ef0f2",
        "type": "ui_group",
        "z": "",
        "name": "环境",
        "tab": "ea08059f.131878",
        "order": 1,
        "disp": true,
        "width": "10",
        "collapse": false
    },
    {
        "id": "7237bf58.e708f",
        "type": "ui_group",
        "z": "",
        "name": "Control",
        "tab": "f90d16f9.323c88",
        "disp": true,
        "width": "6",
        "collapse": false
    },
    {
        "id": "ea08059f.131878",
        "type": "ui_tab",
        "z": "",
        "name": "首页",
        "icon": "fa-bus"
    },
    {
        "id": "f90d16f9.323c88",
        "type": "ui_tab",
        "z": "",
        "name": "智慧旅游",
        "icon": "dashboard"
    }
]
```



##4.3、卫生间自动控制

```javas
[
    {
        "id": "3da2d946.bc3656",
        "type": "subflow",
        "name": "HASS switch off",
        "info": "",
        "in": [
            {
                "x": 320,
                "y": 180,
                "wires": [
                    {
                        "id": "d5ca0cd1.dd752"
                    }
                ]
            }
        ],
        "out": [
            {
                "x": 820,
                "y": 140,
                "wires": [
                    {
                        "id": "b9ac9463.7716a8",
                        "port": 0
                    }
                ]
            },
            {
                "x": 820,
                "y": 220,
                "wires": [
                    {
                        "id": "b9ac9463.7716a8",
                        "port": 1
                    }
                ]
            }
        ]
    },
    {
        "id": "b9ac9463.7716a8",
        "type": "subflow:8c56bc24.a1f0c",
        "z": "3da2d946.bc3656",
        "x": 670,
        "y": 180,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "ec075baf.e5e188",
        "type": "comment",
        "z": "3da2d946.bc3656",
        "name": "ok",
        "info": "",
        "x": 910,
        "y": 140,
        "wires": []
    },
    {
        "id": "a8954113.9b8c6",
        "type": "comment",
        "z": "3da2d946.bc3656",
        "name": "err",
        "info": "",
        "x": 910,
        "y": 220,
        "wires": []
    },
    {
        "id": "d5ca0cd1.dd752",
        "type": "change",
        "z": "3da2d946.bc3656",
        "name": "turn off switch",
        "rules": [
            {
                "t": "set",
                "p": "type",
                "pt": "msg",
                "to": "switch",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "action",
                "pt": "msg",
                "to": "turn_off",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 460,
        "y": 180,
        "wires": [
            [
                "b9ac9463.7716a8"
            ]
        ]
    },
    {
        "id": "8c56bc24.a1f0c",
        "type": "subflow",
        "name": "HASS call service (2)",
        "info": "",
        "in": [
            {
                "x": 260,
                "y": 120,
                "wires": [
                    {
                        "id": "b1fa15a8.99d9c8"
                    }
                ]
            }
        ],
        "out": [
            {
                "x": 980,
                "y": 60,
                "wires": [
                    {
                        "id": "e7df6034.d5cbb",
                        "port": 0
                    }
                ]
            },
            {
                "x": 980,
                "y": 180,
                "wires": [
                    {
                        "id": "7f2d284e.a5f328",
                        "port": 0
                    }
                ]
            }
        ]
    },
    {
        "id": "2b4d1e70.9b74f2",
        "type": "http request",
        "z": "8c56bc24.a1f0c",
        "name": "Call services",
        "method": "POST",
        "ret": "obj",
        "url": "http://192.168.1.198:8123/api/services/{{{type}}}/{{{action}}}",
        "tls": "",
        "x": 590,
        "y": 120,
        "wires": [
            [
                "f5522c97.8fcb2"
            ]
        ]
    },
    {
        "id": "837611bd.68e0c",
        "type": "switch",
        "z": "8c56bc24.a1f0c",
        "name": "is ok",
        "property": "statusCode",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "200",
                "vt": "num"
            },
            {
                "t": "neq",
                "v": "200",
                "vt": "num"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 750,
        "y": 120,
        "wires": [
            [
                "e7df6034.d5cbb"
            ],
            [
                "7f2d284e.a5f328"
            ]
        ]
    },
    {
        "id": "c82bfee6.3f9c3",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "add passwd",
        "rules": [
            {
                "t": "set",
                "p": "headers",
                "pt": "msg",
                "to": "{\"Content-type\": \"application/json\",\"x-ha-access\": \"3332408hjl\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 390,
        "y": 120,
        "wires": [
            [
                "1488cada.8f8345"
            ]
        ]
    },
    {
        "id": "91ad1ea7.b8d86",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "ok",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "ok",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 870,
        "y": 60,
        "wires": [
            []
        ]
    },
    {
        "id": "80ab1389.bb1af",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "err",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "msg.statusCode",
                "tot": "num"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 870,
        "y": 180,
        "wires": [
            []
        ]
    },
    {
        "id": "1488cada.8f8345",
        "type": "http request",
        "z": "8c56bc24.a1f0c",
        "name": "Call services",
        "method": "POST",
        "ret": "obj",
        "url": "http://192.168.1.198:8123/api/services/{{{type}}}/{{{action}}}",
        "tls": "",
        "x": 590,
        "y": 120,
        "wires": [
            [
                "f5522c97.8fcb2"
            ]
        ]
    },
    {
        "id": "f5522c97.8fcb2",
        "type": "switch",
        "z": "8c56bc24.a1f0c",
        "name": "is ok",
        "property": "statusCode",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "200",
                "vt": "num"
            },
            {
                "t": "neq",
                "v": "200",
                "vt": "num"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 750,
        "y": 120,
        "wires": [
            [
                "e7df6034.d5cbb"
            ],
            [
                "7f2d284e.a5f328"
            ]
        ]
    },
    {
        "id": "b1fa15a8.99d9c8",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "add passwd",
        "rules": [
            {
                "t": "set",
                "p": "headers",
                "pt": "msg",
                "to": "{\"Content-type\": \"application/json\",\"x-ha-access\": \"3332408hjl\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 390,
        "y": 120,
        "wires": [
            [
                "1488cada.8f8345"
            ]
        ]
    },
    {
        "id": "e7df6034.d5cbb",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "ok",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "ok",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 870,
        "y": 60,
        "wires": [
            []
        ]
    },
    {
        "id": "7f2d284e.a5f328",
        "type": "change",
        "z": "8c56bc24.a1f0c",
        "name": "err",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "msg.statusCode",
                "tot": "num"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 870,
        "y": 180,
        "wires": [
            []
        ]
    },
    {
        "id": "4ef1f09a.fd264",
        "type": "subflow",
        "name": "HASS switch on",
        "info": "",
        "in": [
            {
                "x": 320,
                "y": 180,
                "wires": [
                    {
                        "id": "98622774.b8df08"
                    }
                ]
            }
        ],
        "out": [
            {
                "x": 820,
                "y": 140,
                "wires": [
                    {
                        "id": "5c0cb66b.74a568",
                        "port": 0
                    }
                ]
            },
            {
                "x": 820,
                "y": 220,
                "wires": [
                    {
                        "id": "5c0cb66b.74a568",
                        "port": 1
                    }
                ]
            }
        ]
    },
    {
        "id": "5c0cb66b.74a568",
        "type": "subflow:8c56bc24.a1f0c",
        "z": "4ef1f09a.fd264",
        "x": 670,
        "y": 180,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "36c383cc.e7d3dc",
        "type": "comment",
        "z": "4ef1f09a.fd264",
        "name": "ok",
        "info": "",
        "x": 910,
        "y": 140,
        "wires": []
    },
    {
        "id": "94b09d6c.83858",
        "type": "comment",
        "z": "4ef1f09a.fd264",
        "name": "err",
        "info": "",
        "x": 910,
        "y": 220,
        "wires": []
    },
    {
        "id": "98622774.b8df08",
        "type": "change",
        "z": "4ef1f09a.fd264",
        "name": "turn on switch",
        "rules": [
            {
                "t": "set",
                "p": "type",
                "pt": "msg",
                "to": "switch",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "action",
                "pt": "msg",
                "to": "turn_on",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 460,
        "y": 180,
        "wires": [
            [
                "5c0cb66b.74a568"
            ]
        ]
    },
    {
        "id": "fff04752.70dab8",
        "type": "subflow",
        "name": "HASS get state",
        "info": "Input: entity_id\nOutput:\n    1. state\n    2. attributes\n    3. last_changed\n    4. last_updated\n    5. entity_id\n    6. whole message",
        "in": [
            {
                "x": 20,
                "y": 140,
                "wires": [
                    {
                        "id": "41aa445.662dbbc"
                    }
                ]
            }
        ],
        "out": [
            {
                "x": 760,
                "y": 40,
                "wires": [
                    {
                        "id": "3c58cafa.49e346",
                        "port": 0
                    }
                ]
            },
            {
                "x": 760,
                "y": 100,
                "wires": [
                    {
                        "id": "3c58cafa.49e346",
                        "port": 1
                    }
                ]
            },
            {
                "x": 760,
                "y": 160,
                "wires": [
                    {
                        "id": "3c58cafa.49e346",
                        "port": 2
                    }
                ]
            },
            {
                "x": 760,
                "y": 220,
                "wires": [
                    {
                        "id": "3c58cafa.49e346",
                        "port": 3
                    }
                ]
            },
            {
                "x": 760,
                "y": 280,
                "wires": [
                    {
                        "id": "3c58cafa.49e346",
                        "port": 4
                    }
                ]
            },
            {
                "x": 760,
                "y": 340,
                "wires": [
                    {
                        "id": "fd76862b.ab76e8",
                        "port": 0
                    }
                ]
            }
        ]
    },
    {
        "id": "fd76862b.ab76e8",
        "type": "http request",
        "z": "fff04752.70dab8",
        "name": "get state",
        "method": "GET",
        "ret": "obj",
        "url": "http://192.168.1.198:8123/api/states/{{{payload}}}",
        "tls": "",
        "x": 340,
        "y": 140,
        "wires": [
            [
                "3c58cafa.49e346"
            ]
        ]
    },
    {
        "id": "296e1086.cbb65",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "state",
        "info": "",
        "x": 850,
        "y": 40,
        "wires": []
    },
    {
        "id": "1dea7df7.6975d2",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "attributes",
        "info": "",
        "x": 860,
        "y": 100,
        "wires": []
    },
    {
        "id": "b342997c.9aeb18",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "last_changed",
        "info": "",
        "x": 870,
        "y": 160,
        "wires": []
    },
    {
        "id": "4b29d3af.108bfc",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "last_updated",
        "info": "",
        "x": 870,
        "y": 220,
        "wires": []
    },
    {
        "id": "59ec842e.41df9c",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "entity_id",
        "info": "",
        "x": 860,
        "y": 280,
        "wires": []
    },
    {
        "id": "167bc7e4.da04e8",
        "type": "comment",
        "z": "fff04752.70dab8",
        "name": "whole msg",
        "info": "",
        "x": 860,
        "y": 340,
        "wires": []
    },
    {
        "id": "3c58cafa.49e346",
        "type": "function",
        "z": "fff04752.70dab8",
        "name": "split msg",
        "func": "var attributes = { payload: msg.payload.attributes };\nvar entity_id = { payload: msg.payload.entity_id };\nvar last_changed = { payload: msg.payload.last_changed };\nvar last_updated = { payload: msg.payload.last_updated };\nvar state = { payload: msg.payload.state };\n\nreturn [state, attributes, last_changed, last_updated, entity_id];",
        "outputs": "5",
        "noerr": 0,
        "x": 520,
        "y": 140,
        "wires": [
            [],
            [],
            [],
            [],
            []
        ]
    },
    {
        "id": "41aa445.662dbbc",
        "type": "change",
        "z": "fff04752.70dab8",
        "name": "add passwd",
        "rules": [
            {
                "t": "set",
                "p": "headers",
                "pt": "msg",
                "to": "{\"Content-type\": \"application/json\",\"x-ha-access\": \"3332408hjl\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 150,
        "y": 140,
        "wires": [
            [
                "fd76862b.ab76e8"
            ]
        ]
    },
    {
        "id": "b03abc38.815bc",
        "type": "debug",
        "z": "505464af.ff161c",
        "name": "err",
        "active": true,
        "console": "false",
        "complete": "payload",
        "x": 1410,
        "y": 880,
        "wires": []
    },
    {
        "id": "b22d65fe.8ac168",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "err",
        "links": [
            "ae4cdb80.12f0b8",
            "bb01dbb2.9a5038",
            "ccbabc83.8c38f",
            "9a7ea8a9.5ef238"
        ],
        "x": 1295,
        "y": 880,
        "wires": [
            [
                "b03abc38.815bc"
            ]
        ]
    },
    {
        "id": "40773e76.dd86c",
        "type": "debug",
        "z": "505464af.ff161c",
        "name": "debug",
        "active": true,
        "console": "false",
        "complete": "true",
        "x": 1410,
        "y": 840,
        "wires": []
    },
    {
        "id": "f0500936.d6a328",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "debug",
        "links": [
            "7d4c706b.fb8e4",
            "89f16a25.5cf068",
            "962a003a.f0b8e",
            "1ed6564e.05d7da",
            "14296506.14ad1b",
            "33fd4575.22e20a",
            "a8ba9309.1eef1",
            "4e6102.e50f0f",
            "f00c56aa.5a70b8",
            "27478ccc.718454",
            "a23d5dd0.e7b",
            "6f0c197b.389928",
            "893c47b1.8906d8",
            "5366750f.70627c",
            "88ce2cbd.2cedb",
            "401a5a8d.41c504",
            "eaa67573.f7a648",
            "318ce8b7.58dc38",
            "9ad75fc0.f0bf2",
            "b8fcafc3.a2e21",
            "5ac44613.bc2788",
            "e732d2de.7171b"
        ],
        "x": 1295,
        "y": 840,
        "wires": [
            [
                "40773e76.dd86c"
            ]
        ]
    },
    {
        "id": "3b5995.e69f166c",
        "type": "inject",
        "z": "505464af.ff161c",
        "name": "洗手间门",
        "topic": "",
        "payload": "binary_sensor.door_window_sensor_158d0001549234",
        "payloadType": "str",
        "repeat": "1",
        "crontab": "",
        "once": true,
        "x": 110,
        "y": 100,
        "wires": [
            [
                "2acfa7b2.409a28"
            ]
        ]
    },
    {
        "id": "2acfa7b2.409a28",
        "type": "subflow:fff04752.70dab8",
        "z": "505464af.ff161c",
        "name": "",
        "x": 340,
        "y": 100,
        "wires": [
            [
                "b3f19ae3.34a8a8"
            ],
            [],
            [],
            [],
            [],
            []
        ]
    },
    {
        "id": "b3f19ae3.34a8a8",
        "type": "rbe",
        "z": "505464af.ff161c",
        "name": "有变化",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "x": 530,
        "y": 100,
        "wires": [
            [
                "226ef727.ecf808"
            ]
        ]
    },
    {
        "id": "226ef727.ecf808",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "已打开",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "on",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "off",
                "vt": "str"
            }
        ],
        "checkall": "false",
        "outputs": 2,
        "x": 670,
        "y": 100,
        "wires": [
            [
                "1aab3316.f2bd8d"
            ],
            [
                "cdf34d72.9f956"
            ]
        ]
    },
    {
        "id": "1aab3316.f2bd8d",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "开门",
        "rules": [
            {
                "t": "set",
                "p": "door_open",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            },
            {
                "t": "set",
                "p": "no_motion_delay_s",
                "pt": "flow",
                "to": "120",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 80,
        "wires": [
            [
                "c47b2138.1381e",
                "a7476eb.812b39"
            ]
        ]
    },
    {
        "id": "8c8e918b.2ea14",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "洗手间吸顶灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "{\"entity_id\": \"switch.wall_switch_left_158d00010ee3fb\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 180,
        "y": 460,
        "wires": [
            [
                "2bd0ec78.546604"
            ]
        ]
    },
    {
        "id": "40415f2.bc566a",
        "type": "subflow:4ef1f09a.fd264",
        "z": "505464af.ff161c",
        "name": "",
        "x": 820,
        "y": 460,
        "wires": [
            [
                "7d4c706b.fb8e4",
                "68580a2.c669df4"
            ],
            [
                "ccbabc83.8c38f",
                "60e62c12.881bc4"
            ]
        ]
    },
    {
        "id": "2bd0ec78.546604",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "灯可以操作",
        "property": "light_lock",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 370,
        "y": 460,
        "wires": [
            [
                "e826e286.5b4e3"
            ]
        ]
    },
    {
        "id": "91337135.50aeb",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "开灯",
        "links": [
            "d2e282f7.6547b",
            "e0dee651.1ac4b8"
        ],
        "x": 55,
        "y": 460,
        "wires": [
            [
                "8c8e918b.2ea14"
            ]
        ]
    },
    {
        "id": "9dc57057.2c65e",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "洗手间吸顶灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "{\"entity_id\": \"switch.wall_switch_left_158d00010ee3fb\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 180,
        "y": 540,
        "wires": [
            [
                "ed94bbca.83d998"
            ]
        ]
    },
    {
        "id": "ed94bbca.83d998",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "灯可以操作",
        "property": "light_lock",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 370,
        "y": 540,
        "wires": [
            [
                "1db5d405.60871c"
            ]
        ]
    },
    {
        "id": "bdbe50d5.9f595",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "锁定灯操作",
        "rules": [
            {
                "t": "set",
                "p": "light_lock",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 690,
        "y": 360,
        "wires": [
            [
                "b88da1cc.4c10c"
            ]
        ]
    },
    {
        "id": "b88da1cc.4c10c",
        "type": "delay",
        "z": "505464af.ff161c",
        "name": "延时10秒",
        "pauseType": "delay",
        "timeout": "10",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "x": 860,
        "y": 360,
        "wires": [
            [
                "eee2222.c9ac5e"
            ]
        ]
    },
    {
        "id": "eee2222.c9ac5e",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "解锁灯操作",
        "rules": [
            {
                "t": "set",
                "p": "light_lock",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1030,
        "y": 360,
        "wires": [
            []
        ]
    },
    {
        "id": "42d0e437.5ee0ac",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "关灯",
        "links": [
            "70398a57.513e94",
            "d0dc4178.18111",
            "c4e7e83c.67b1a8",
            "95fdb4bb.269ba8",
            "ebb2429a.8a199"
        ],
        "x": 55,
        "y": 540,
        "wires": [
            [
                "9dc57057.2c65e"
            ]
        ]
    },
    {
        "id": "a2a2a51a.ca1668",
        "type": "subflow:3da2d946.bc3656",
        "z": "505464af.ff161c",
        "x": 820,
        "y": 540,
        "wires": [
            [
                "89f16a25.5cf068",
                "7acf935.211bf6c"
            ],
            [
                "9a7ea8a9.5ef238",
                "454774b8.89335c"
            ]
        ]
    },
    {
        "id": "36611dc2.4003b2",
        "type": "inject",
        "z": "505464af.ff161c",
        "name": "洗手间灯",
        "topic": "",
        "payload": "switch.wall_switch_left_158d00010ee3fb",
        "payloadType": "str",
        "repeat": "1",
        "crontab": "",
        "once": true,
        "x": 110,
        "y": 340,
        "wires": [
            [
                "256f4fda.1a4b4"
            ]
        ]
    },
    {
        "id": "256f4fda.1a4b4",
        "type": "subflow:fff04752.70dab8",
        "z": "505464af.ff161c",
        "name": "",
        "x": 340,
        "y": 340,
        "wires": [
            [
                "2bbbe470.45ddbc"
            ],
            [],
            [],
            [],
            [],
            []
        ]
    },
    {
        "id": "2bbbe470.45ddbc",
        "type": "rbe",
        "z": "505464af.ff161c",
        "name": "有变化",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "x": 530,
        "y": 360,
        "wires": [
            [
                "bdbe50d5.9f595",
                "98a998ae.f96bc8"
            ]
        ]
    },
    {
        "id": "b7495b57.f9d048",
        "type": "inject",
        "z": "505464af.ff161c",
        "name": "洗手间人体传感器",
        "topic": "",
        "payload": "binary_sensor.motion_sensor_158d0000f44617",
        "payloadType": "str",
        "repeat": "1",
        "crontab": "",
        "once": true,
        "x": 130,
        "y": 220,
        "wires": [
            [
                "43d2af86.d7a16"
            ]
        ]
    },
    {
        "id": "43d2af86.d7a16",
        "type": "subflow:fff04752.70dab8",
        "z": "505464af.ff161c",
        "name": "",
        "x": 340,
        "y": 220,
        "wires": [
            [
                "49ee2796.5d48c8"
            ],
            [
                "25a77ee6.a0d952"
            ],
            [],
            [],
            [],
            []
        ]
    },
    {
        "id": "49ee2796.5d48c8",
        "type": "rbe",
        "z": "505464af.ff161c",
        "name": "有变化",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "x": 530,
        "y": 200,
        "wires": [
            [
                "8de50b09.db0eb8"
            ]
        ]
    },
    {
        "id": "8de50b09.db0eb8",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "已打开",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "on",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "off",
                "vt": "str"
            }
        ],
        "checkall": "false",
        "outputs": 2,
        "x": 670,
        "y": 200,
        "wires": [
            [
                "a3013815.6b7a28"
            ],
            [
                "5b95a90d.f35048"
            ]
        ]
    },
    {
        "id": "2a58085a.21cc98",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "有人",
        "rules": [
            {
                "t": "set",
                "p": "human_moving",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 930,
        "y": 160,
        "wires": [
            [
                "ebfd9ced.33c51",
                "f781dde9.0d39f"
            ]
        ]
    },
    {
        "id": "e0a57181.a5e26",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "x分钟没有变化",
        "property": "payload['No motion since']",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "no_motion_delay_s",
                "vt": "flow"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 640,
        "y": 240,
        "wires": [
            [
                "b1af1ea1.08177"
            ]
        ]
    },
    {
        "id": "c2e547e4.61ca58",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "无人",
        "rules": [
            {
                "t": "set",
                "p": "human_moving",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 930,
        "y": 240,
        "wires": [
            [
                "9749fef.c7809",
                "cae7bb1f.e158a8"
            ]
        ]
    },
    {
        "id": "c47b2138.1381e",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "洗手间门由关到开",
        "links": [
            "5bb34b0c.a778c4",
            "30f9709f.9fda5"
        ],
        "x": 1015,
        "y": 80,
        "wires": []
    },
    {
        "id": "30f9709f.9fda5",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "洗手间门由关到开",
        "links": [
            "c47b2138.1381e"
        ],
        "x": 55,
        "y": 1040,
        "wires": [
            [
                "8523c0d4.268fa"
            ]
        ]
    },
    {
        "id": "8523c0d4.268fa",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "无人",
        "property": "human_moving",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 170,
        "y": 1040,
        "wires": [
            [
                "d2e282f7.6547b",
                "538ff61f.6f6138"
            ]
        ]
    },
    {
        "id": "d2e282f7.6547b",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "开灯",
        "links": [
            "91337135.50aeb"
        ],
        "x": 275,
        "y": 1040,
        "wires": []
    },
    {
        "id": "9f7cb8d4.3a1408",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "有人移动",
        "links": [
            "ebfd9ced.33c51"
        ],
        "x": 55,
        "y": 640,
        "wires": [
            [
                "40f2260a.ac1538"
            ]
        ]
    },
    {
        "id": "e0dee651.1ac4b8",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "开灯",
        "links": [
            "91337135.50aeb"
        ],
        "x": 335,
        "y": 640,
        "wires": []
    },
    {
        "id": "98a998ae.f96bc8",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "已打开",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "on",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "off",
                "vt": "str"
            }
        ],
        "checkall": "false",
        "outputs": 2,
        "x": 670,
        "y": 300,
        "wires": [
            [
                "a3e833a1.984f8"
            ],
            [
                "ac553310.2df98"
            ]
        ]
    },
    {
        "id": "a3e833a1.984f8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "灯已开",
        "rules": [
            {
                "t": "set",
                "p": "light_on",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 280,
        "wires": [
            [
                "37769f69.79346",
                "427c269.0165ed8"
            ]
        ]
    },
    {
        "id": "ac553310.2df98",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "灯已关",
        "rules": [
            {
                "t": "set",
                "p": "light_on",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 320,
        "wires": [
            [
                "e3c68925.3860f8"
            ]
        ]
    },
    {
        "id": "1db5d405.60871c",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "灯已开",
        "property": "light_on",
        "propertyType": "flow",
        "rules": [
            {
                "t": "true"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 570,
        "y": 540,
        "wires": [
            [
                "a2a2a51a.ca1668"
            ]
        ]
    },
    {
        "id": "e826e286.5b4e3",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "灯已关",
        "property": "light_on",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 570,
        "y": 460,
        "wires": [
            [
                "40415f2.bc566a"
            ]
        ]
    },
    {
        "id": "7d4c706b.fb8e4",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "debug",
        "links": [
            "f0500936.d6a328"
        ],
        "x": 955,
        "y": 440,
        "wires": []
    },
    {
        "id": "89f16a25.5cf068",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "debug",
        "links": [
            "f0500936.d6a328"
        ],
        "x": 955,
        "y": 520,
        "wires": []
    },
    {
        "id": "ccbabc83.8c38f",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "err",
        "links": [
            "b22d65fe.8ac168"
        ],
        "x": 955,
        "y": 480,
        "wires": []
    },
    {
        "id": "9a7ea8a9.5ef238",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "err",
        "links": [
            "b22d65fe.8ac168"
        ],
        "x": 955,
        "y": 560,
        "wires": []
    },
    {
        "id": "f7a6a38c.278df",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "无人移动",
        "links": [
            "9749fef.c7809"
        ],
        "x": 55,
        "y": 960,
        "wires": [
            [
                "13ff4ffe.3a411"
            ]
        ]
    },
    {
        "id": "8626ed95.f58cc",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "不需要等待开门",
        "property": "waiting_door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 400,
        "y": 960,
        "wires": [
            [
                "c4e7e83c.67b1a8",
                "fcf09f9f.84315"
            ]
        ]
    },
    {
        "id": "bc619863.972ef8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "等待开门",
        "rules": [
            {
                "t": "set",
                "p": "waiting_door_open",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1100,
        "y": 880,
        "wires": [
            [
                "65479c24.013f74"
            ]
        ]
    },
    {
        "id": "5f9ae2af.a3159c",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "等到开门",
        "rules": [
            {
                "t": "set",
                "p": "waiting_door_open",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 180,
        "y": 800,
        "wires": [
            [
                "1de29443.3af32c"
            ]
        ]
    },
    {
        "id": "c4e7e83c.67b1a8",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "关灯",
        "links": [
            "42d0e437.5ee0ac"
        ],
        "x": 535,
        "y": 960,
        "wires": []
    },
    {
        "id": "ebfd9ced.33c51",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "人体传感器从无到有",
        "links": [
            "9f7cb8d4.3a1408",
            "12a9cc10.949ee4"
        ],
        "x": 1015,
        "y": 160,
        "wires": []
    },
    {
        "id": "12a9cc10.949ee4",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "有人移动",
        "links": [
            "ebfd9ced.33c51"
        ],
        "x": 55,
        "y": 880,
        "wires": [
            [
                "40a6f23c.4b5bec"
            ]
        ]
    },
    {
        "id": "74026e68.49101",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "不需要等待开门",
        "property": "waiting_door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 400,
        "y": 880,
        "wires": [
            [
                "9b782b50.7cdc28"
            ]
        ]
    },
    {
        "id": "40a6f23c.4b5bec",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "洗手间门已关",
        "property": "door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 200,
        "y": 880,
        "wires": [
            [
                "74026e68.49101"
            ]
        ]
    },
    {
        "id": "8070c9a5.e34068",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "洗手间吸顶灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "{\"entity_id\": \"switch.wall_switch_left_158d00010ee3fb\"}",
                "tot": "json"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 740,
        "y": 900,
        "wires": [
            [
                "b92dc916.3b09c8"
            ]
        ]
    },
    {
        "id": "b92dc916.3b09c8",
        "type": "subflow:4ef1f09a.fd264",
        "z": "505464af.ff161c",
        "name": "",
        "x": 920,
        "y": 900,
        "wires": [
            [
                "bc619863.972ef8"
            ],
            []
        ]
    },
    {
        "id": "9b782b50.7cdc28",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "灯状态",
        "property": "light_on",
        "propertyType": "flow",
        "rules": [
            {
                "t": "true"
            },
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 2,
        "x": 570,
        "y": 880,
        "wires": [
            [
                "bc619863.972ef8"
            ],
            [
                "8070c9a5.e34068"
            ]
        ]
    },
    {
        "id": "5bb34b0c.a778c4",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "洗手间门由关到开",
        "links": [
            "c47b2138.1381e",
            "e7a25f48.78ea2"
        ],
        "x": 55,
        "y": 800,
        "wires": [
            [
                "5f9ae2af.a3159c"
            ]
        ]
    },
    {
        "id": "232bdcaa.9fbe64",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "获取洗手间门状态",
        "info": "",
        "x": 130,
        "y": 40,
        "wires": []
    },
    {
        "id": "70388d0d.fcab34",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "获取人体传感器状态",
        "info": "",
        "x": 130,
        "y": 160,
        "wires": []
    },
    {
        "id": "e2c12611.ebac38",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "获取洗手间灯状态",
        "info": "",
        "x": 130,
        "y": 280,
        "wires": []
    },
    {
        "id": "72f28608.45c1e8",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "控制洗手间灯",
        "info": "",
        "x": 110,
        "y": 420,
        "wires": []
    },
    {
        "id": "1074cd09.f7a763",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "日志",
        "info": "",
        "x": 1330,
        "y": 760,
        "wires": []
    },
    {
        "id": "c9b5e569.bb6d78",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "开门, 洗手间无人, 亮灯",
        "info": "",
        "x": 140,
        "y": 1000,
        "wires": []
    },
    {
        "id": "da2d844a.3db578",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "有人移动, 洗手间门常开, 开灯",
        "info": "",
        "x": 160,
        "y": 600,
        "wires": []
    },
    {
        "id": "c71fa991.b5dd48",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "有人移动, 洗手间门常关, 开灯, 并且保持到门开",
        "info": "",
        "x": 210,
        "y": 760,
        "wires": []
    },
    {
        "id": "5063f30d.720d5c",
        "type": "file",
        "z": "505464af.ff161c",
        "name": "log",
        "filename": "/data/卫生间灯控制/log.txt",
        "appendNewline": true,
        "createDir": true,
        "overwriteFile": "false",
        "x": 1570,
        "y": 800,
        "wires": []
    },
    {
        "id": "cdf34d72.9f956",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "关门",
        "rules": [
            {
                "t": "set",
                "p": "door_open",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            },
            {
                "t": "set",
                "p": "no_motion_delay_s",
                "pt": "flow",
                "to": "600",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 120,
        "wires": [
            [
                "8ab3fead.0712a",
                "cc8cfaba.3169f8"
            ]
        ]
    },
    {
        "id": "8f440d2a.69db8",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "洗手间门由开到关",
        "links": [
            "8ab3fead.0712a"
        ],
        "x": 55,
        "y": 1120,
        "wires": [
            [
                "c9a71632.cd8c38"
            ]
        ]
    },
    {
        "id": "c9a71632.cd8c38",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "无人",
        "property": "human_moving",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 170,
        "y": 1120,
        "wires": [
            [
                "95fdb4bb.269ba8",
                "c3cb3613.ff2868"
            ]
        ]
    },
    {
        "id": "95fdb4bb.269ba8",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "关灯",
        "links": [
            "42d0e437.5ee0ac"
        ],
        "x": 275,
        "y": 1120,
        "wires": []
    },
    {
        "id": "2ada695a.9a5056",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "关门, 洗手间无人, 关灯",
        "info": "",
        "x": 140,
        "y": 1080,
        "wires": []
    },
    {
        "id": "8ab3fead.0712a",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "洗手间门由开到关",
        "links": [
            "8f440d2a.69db8"
        ],
        "x": 1015,
        "y": 120,
        "wires": []
    },
    {
        "id": "b1af1ea1.08177",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "检测有人",
        "property": "detect_human_moving",
        "propertyType": "flow",
        "rules": [
            {
                "t": "true"
            },
            {
                "t": "false"
            }
        ],
        "checkall": "false",
        "outputs": 2,
        "x": 800,
        "y": 240,
        "wires": [
            [],
            [
                "c2e547e4.61ca58"
            ]
        ]
    },
    {
        "id": "d39f2164.5689d",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "无人移动, 洗手间门常开, 关灯",
        "info": "",
        "x": 160,
        "y": 680,
        "wires": []
    },
    {
        "id": "9749fef.c7809",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "人体传感器从有到无",
        "links": [
            "86caf166.18173",
            "f7a6a38c.278df"
        ],
        "x": 1015,
        "y": 240,
        "wires": []
    },
    {
        "id": "40f2260a.ac1538",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "洗手间门已开",
        "property": "door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "true"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 200,
        "y": 640,
        "wires": [
            [
                "e0dee651.1ac4b8",
                "7dc2bc81.ab21e4"
            ]
        ]
    },
    {
        "id": "86caf166.18173",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "无人移动",
        "links": [
            "9749fef.c7809"
        ],
        "x": 55,
        "y": 720,
        "wires": [
            [
                "36a5caa4.c572e6"
            ]
        ]
    },
    {
        "id": "ebb2429a.8a199",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "关灯",
        "links": [
            "42d0e437.5ee0ac"
        ],
        "x": 335,
        "y": 720,
        "wires": []
    },
    {
        "id": "36a5caa4.c572e6",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "洗手间门已开",
        "property": "door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "true"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 200,
        "y": 720,
        "wires": [
            [
                "ebb2429a.8a199",
                "abf6c9f4.7472b8"
            ]
        ]
    },
    {
        "id": "4e805df9.33f344",
        "type": "comment",
        "z": "505464af.ff161c",
        "name": "无人移动, 洗手间门常关, 不需要等待开门时关灯",
        "info": "",
        "x": 220,
        "y": 920,
        "wires": []
    },
    {
        "id": "13ff4ffe.3a411",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "洗手间门已关",
        "property": "door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 200,
        "y": 960,
        "wires": [
            [
                "8626ed95.f58cc"
            ]
        ]
    },
    {
        "id": "37769f69.79346",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "灯已打开",
        "links": [
            "77d24ea0.b2729"
        ],
        "x": 1015,
        "y": 280,
        "wires": []
    },
    {
        "id": "77d24ea0.b2729",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "灯已打开",
        "links": [
            "37769f69.79346"
        ],
        "x": 55,
        "y": 840,
        "wires": [
            [
                "2e1a33d6.48cf9c"
            ]
        ]
    },
    {
        "id": "2e1a33d6.48cf9c",
        "type": "switch",
        "z": "505464af.ff161c",
        "name": "洗手间门已关",
        "property": "door_open",
        "propertyType": "flow",
        "rules": [
            {
                "t": "false"
            }
        ],
        "checkall": "true",
        "outputs": 1,
        "x": 200,
        "y": 840,
        "wires": [
            [
                "c4fb881a.246408"
            ]
        ]
    },
    {
        "id": "c4fb881a.246408",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "等待开门",
        "rules": [
            {
                "t": "set",
                "p": "waiting_door_open",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 380,
        "y": 840,
        "wires": [
            [
                "3f336d84.186832"
            ]
        ]
    },
    {
        "id": "a3013815.6b7a28",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "检测有人",
        "rules": [
            {
                "t": "set",
                "p": "detect_human_moving",
                "pt": "flow",
                "to": "true",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 800,
        "y": 160,
        "wires": [
            [
                "2a58085a.21cc98"
            ]
        ]
    },
    {
        "id": "5b95a90d.f35048",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "检测无人",
        "rules": [
            {
                "t": "set",
                "p": "detect_human_moving",
                "pt": "flow",
                "to": "false",
                "tot": "bool"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 800,
        "y": 200,
        "wires": [
            []
        ]
    },
    {
        "id": "25a77ee6.a0d952",
        "type": "rbe",
        "z": "505464af.ff161c",
        "name": "有变化",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "x": 490,
        "y": 240,
        "wires": [
            [
                "e0a57181.a5e26"
            ]
        ]
    },
    {
        "id": "98d8168f.2bbe98",
        "type": "link in",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "68580a2.c669df4",
            "60e62c12.881bc4",
            "7acf935.211bf6c",
            "454774b8.89335c",
            "2b27281e.14cea8",
            "ecf2acd9.5031c",
            "1f54d42e.ae590c",
            "533ccf93.eeb75",
            "1575ea12.dc6b76",
            "5ce0d119.1813e",
            "1cdd155c.18686b",
            "46af0fc1.78305",
            "73230cc2.ca9344",
            "aba0d25a.42b5b",
            "313779d0.6049c6",
            "ac2f949.f199068",
            "57a87b42.5e7c54",
            "9ef08060.adf89"
        ],
        "x": 1295,
        "y": 800,
        "wires": [
            [
                "4d5b8d62.d235e4"
            ]
        ]
    },
    {
        "id": "68580a2.c669df4",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 995,
        "y": 440,
        "wires": []
    },
    {
        "id": "60e62c12.881bc4",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 995,
        "y": 480,
        "wires": []
    },
    {
        "id": "7acf935.211bf6c",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 995,
        "y": 520,
        "wires": []
    },
    {
        "id": "454774b8.89335c",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 995,
        "y": 560,
        "wires": []
    },
    {
        "id": "7dc2bc81.ab21e4",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "有人移动洗手间门常开开灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "有人移动洗手间门常开开灯",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 500,
        "y": 640,
        "wires": [
            [
                "2b27281e.14cea8"
            ]
        ]
    },
    {
        "id": "2b27281e.14cea8",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 675,
        "y": 640,
        "wires": []
    },
    {
        "id": "abf6c9f4.7472b8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "无人移动洗手间门常开关灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "无人移动洗手间门常开关灯",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 500,
        "y": 720,
        "wires": [
            [
                "ecf2acd9.5031c"
            ]
        ]
    },
    {
        "id": "ecf2acd9.5031c",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 675,
        "y": 720,
        "wires": []
    },
    {
        "id": "1de29443.3af32c",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "等到开门",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "等到开门",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 440,
        "y": 800,
        "wires": [
            [
                "1f54d42e.ae590c"
            ]
        ]
    },
    {
        "id": "1f54d42e.ae590c",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 675,
        "y": 800,
        "wires": []
    },
    {
        "id": "3f336d84.186832",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "人为开灯, 等待开门",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "人为开灯, 等待开门",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 610,
        "y": 840,
        "wires": [
            [
                "533ccf93.eeb75"
            ]
        ]
    },
    {
        "id": "533ccf93.eeb75",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 815,
        "y": 840,
        "wires": []
    },
    {
        "id": "65479c24.013f74",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "检测到无人, 等待开门",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "检测到无人, 等待开门",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1060,
        "y": 960,
        "wires": [
            [
                "1575ea12.dc6b76"
            ]
        ]
    },
    {
        "id": "1575ea12.dc6b76",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1195,
        "y": 960,
        "wires": []
    },
    {
        "id": "fcf09f9f.84315",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "无人移动 洗手间门常关关灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "无人移动 洗手间门常关关灯",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 700,
        "y": 960,
        "wires": [
            [
                "5ce0d119.1813e"
            ]
        ]
    },
    {
        "id": "5ce0d119.1813e",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 875,
        "y": 960,
        "wires": []
    },
    {
        "id": "538ff61f.6f6138",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "开门,无人,亮灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "开门,无人,亮灯",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 400,
        "y": 1040,
        "wires": [
            [
                "1cdd155c.18686b"
            ]
        ]
    },
    {
        "id": "1cdd155c.18686b",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 615,
        "y": 1040,
        "wires": []
    },
    {
        "id": "c3cb3613.ff2868",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "关门,无人,关灯",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "关门,无人,关灯",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 400,
        "y": 1120,
        "wires": [
            [
                "46af0fc1.78305"
            ]
        ]
    },
    {
        "id": "46af0fc1.78305",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 615,
        "y": 1120,
        "wires": []
    },
    {
        "id": "a7476eb.812b39",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "开门",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "开门",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 80,
        "wires": [
            [
                "73230cc2.ca9344"
            ]
        ]
    },
    {
        "id": "73230cc2.ca9344",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 80,
        "wires": []
    },
    {
        "id": "cc8cfaba.3169f8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "关门",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "关门",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 120,
        "wires": [
            [
                "aba0d25a.42b5b"
            ]
        ]
    },
    {
        "id": "aba0d25a.42b5b",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 120,
        "wires": []
    },
    {
        "id": "f781dde9.0d39f",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "有人",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "有人",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 160,
        "wires": [
            [
                "313779d0.6049c6"
            ]
        ]
    },
    {
        "id": "313779d0.6049c6",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 160,
        "wires": []
    },
    {
        "id": "cae7bb1f.e158a8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "无人",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "无人",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 240,
        "wires": [
            [
                "ac2f949.f199068"
            ]
        ]
    },
    {
        "id": "ac2f949.f199068",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 240,
        "wires": []
    },
    {
        "id": "427c269.0165ed8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "灯开",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "灯开",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 280,
        "wires": [
            [
                "57a87b42.5e7c54"
            ]
        ]
    },
    {
        "id": "57a87b42.5e7c54",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 280,
        "wires": []
    },
    {
        "id": "e3c68925.3860f8",
        "type": "change",
        "z": "505464af.ff161c",
        "name": "灯关",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "灯关",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 320,
        "wires": [
            [
                "9ef08060.adf89"
            ]
        ]
    },
    {
        "id": "9ef08060.adf89",
        "type": "link out",
        "z": "505464af.ff161c",
        "name": "log",
        "links": [
            "98d8168f.2bbe98",
            "8a5fadb4.56c42"
        ],
        "x": 1235,
        "y": 320,
        "wires": []
    },
    {
        "id": "4d5b8d62.d235e4",
        "type": "function",
        "z": "505464af.ff161c",
        "name": "增加时间戳",
        "func": "if ( !msg.timestamp ) msg.timestamp = Math.round(+new Date());\n\nvar dt = new Date(msg.timestamp);\nmsg.payload = dt.getFullYear()+\"/\"+(dt.getMonth()+1)+\"/\"+dt.getDate()+\" \"+dt.getHours()+\":\"+dt.getMinutes()+\":\"+dt.getSeconds()+\".\"+dt.getMilliseconds()+\"  \"+msg.payload;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1430,
        "y": 800,
        "wires": [
            [
                "5063f30d.720d5c"
            ]
        ]
    }
]
```

## 4.4、高级Dashboard

```javascript
[
    {
        "id": "18b68040.d9246",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 2,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "Bid Price",
        "format": "{{msg.payload.bidPrice}}",
        "layout": "row-spread",
        "x": 900,
        "y": 100,
        "wires": []
    },
    {
        "id": "8907eb9c.b4f468",
        "type": "ui_chart",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 3,
        "width": "18",
        "height": "10",
        "label": "",
        "chartType": "line",
        "legend": "true",
        "xformat": "auto",
        "interpolate": "linear",
        "nodata": "Click on a crypto or provide a ticker pair",
        "dot": true,
        "ymin": "",
        "ymax": "",
        "removeOlder": "30",
        "removeOlderPoints": "",
        "removeOlderUnit": "86400",
        "cutout": 0,
        "useOneColor": false,
        "colors": [
            "#1f77b4",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#d62728",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "useOldStyle": false,
        "x": 990,
        "y": 640,
        "wires": [
            [],
            []
        ]
    },
    {
        "id": "1fca9467.e7501c",
        "type": "binance-get-candlesticks",
        "z": "8cbdda14.b996b8",
        "name": "",
        "ticker": "",
        "interval": "1d",
        "limit": "30",
        "startTime": "",
        "endTime": "",
        "x": 960,
        "y": 520,
        "wires": [
            [
                "1f9910f5.a643ff"
            ]
        ]
    },
    {
        "id": "1f9910f5.a643ff",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "built chart data",
        "func": "\nvar series = [msg.topic]\nvar data = msg.payload.map(function (d) {\n return {\n x: d[0],\n y: d[4] // close price\n }\n});\n\nmsg.payload = [{\n series: series,\n data: [data]\n}]\n\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 960,
        "y": 580,
        "wires": [
            [
                "8907eb9c.b4f468"
            ]
        ]
    },
    {
        "id": "5eae3dbd.e743f4",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "set values for API",
        "func": "if (typeof msg.payload === 'string') {\n flow.set(\"selectedTickerPair\", msg.payload);\n}\nmsg.topic = flow.get(\"selectedTickerPair\");\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 490,
        "y": 240,
        "wires": [
            [
                "773b511e.cc6d",
                "46f1238e.152edc",
                "c5ee73a8.47dcd",
                "c71a5eb7.0712d"
            ]
        ]
    },
    {
        "id": "a55e2aaa.656d78",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 2,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Refresh",
        "color": "",
        "bgcolor": "",
        "icon": "refresh",
        "payload": "selectedTickerPair",
        "payloadType": "flow",
        "topic": "",
        "x": 200,
        "y": 100,
        "wires": [
            [
                "a2e9524.1e63bb",
                "2c8481a.f4d337e"
            ]
        ]
    },
    {
        "id": "78486c21.a55464",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 6,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Ethereum",
        "color": "",
        "bgcolor": "#555555",
        "icon": "",
        "payload": "ETHBTC",
        "payloadType": "str",
        "topic": "",
        "x": 200,
        "y": 180,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "d7cccc16.b7127",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 7,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Neo",
        "color": "",
        "bgcolor": "#58be02",
        "icon": "",
        "payload": "NEOBTC",
        "payloadType": "str",
        "topic": "",
        "x": 190,
        "y": 220,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "9ec82e2.10107d",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 4,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "BTC",
        "color": "",
        "bgcolor": "#ff8e13",
        "icon": "",
        "payload": "BTCUSDT",
        "payloadType": "str",
        "topic": "",
        "x": 190,
        "y": 140,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "d7d660c3.3afdc",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 5,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Litecoin",
        "color": "",
        "bgcolor": "#bebebe",
        "icon": "",
        "payload": "LTCBTC",
        "payloadType": "str",
        "topic": "",
        "x": 200,
        "y": 260,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "773b511e.cc6d",
        "type": "binance-get-book-ticker",
        "z": "8cbdda14.b996b8",
        "name": "",
        "ticker": "",
        "x": 720,
        "y": 120,
        "wires": [
            [
                "18b68040.d9246",
                "eaebe162.be797"
            ]
        ]
    },
    {
        "id": "eaebe162.be797",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 3,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "Ask Price",
        "format": "{{msg.payload.askPrice}}",
        "layout": "row-spread",
        "x": 900,
        "y": 140,
        "wires": []
    },
    {
        "id": "a2e9524.1e63bb",
        "type": "ui_text_input",
        "z": "8cbdda14.b996b8",
        "name": "",
        "label": "Ticker pair",
        "group": "49b58639.d95708",
        "order": 1,
        "width": 0,
        "height": 0,
        "passthru": true,
        "mode": "text",
        "delay": 300,
        "topic": "",
        "x": 470,
        "y": 160,
        "wires": [
            [
                "ab114c85.0d99e"
            ]
        ]
    },
    {
        "id": "46f1238e.152edc",
        "type": "binance-get-day-stats",
        "z": "8cbdda14.b996b8",
        "name": "",
        "ticker": "",
        "x": 710,
        "y": 220,
        "wires": [
            [
                "659707c.e846cf8",
                "46e5f906.9048d8",
                "ec642ea3.0dfec",
                "cc4b6a30.29fe48"
            ]
        ]
    },
    {
        "id": "b3818b0b.754488",
        "type": "catch",
        "z": "8cbdda14.b996b8",
        "name": "",
        "scope": null,
        "x": 200,
        "y": 680,
        "wires": [
            [
                "80b0604e.7023c",
                "486fe1d9.0419a"
            ]
        ]
    },
    {
        "id": "4cccdfc1.fc7ca",
        "type": "ui_toast",
        "z": "8cbdda14.b996b8",
        "position": "top right",
        "displayTime": "3",
        "highlight": "",
        "outputs": 0,
        "ok": "OK",
        "cancel": "",
        "topic": "",
        "name": "",
        "x": 650,
        "y": 680,
        "wires": []
    },
    {
        "id": "80b0604e.7023c",
        "type": "debug",
        "z": "8cbdda14.b996b8",
        "name": "",
        "active": true,
        "console": false,
        "complete": "error",
        "x": 340,
        "y": 720,
        "wires": []
    },
    {
        "id": "486fe1d9.0419a",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "",
        "func": "msg.topic = \"API Error\";\nif (msg.error) {\n msg.payload = msg.error.message\n} else {\n msg.payload = \"\"\n}\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 330,
        "y": 680,
        "wires": [
            [
                "e99f75a3.e130e8"
            ]
        ]
    },
    {
        "id": "659707c.e846cf8",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 4,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "24 hr Low",
        "format": "{{msg.payload.lowPrice}}",
        "layout": "row-spread",
        "x": 900,
        "y": 180,
        "wires": []
    },
    {
        "id": "46e5f906.9048d8",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 5,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "24 hr high",
        "format": "{{msg.payload.highPrice}}",
        "layout": "row-spread",
        "x": 900,
        "y": 220,
        "wires": []
    },
    {
        "id": "ec642ea3.0dfec",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 6,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "24 hr Price % change",
        "format": "{{msg.payload.priceChangePercent}}%",
        "layout": "row-spread",
        "x": 940,
        "y": 260,
        "wires": []
    },
    {
        "id": "cc4b6a30.29fe48",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 7,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "24 hr volume",
        "format": "{{msg.payload.volume}}",
        "layout": "row-spread",
        "x": 910,
        "y": 300,
        "wires": []
    },
    {
        "id": "423e0222.d8831c",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 8,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Ripple",
        "color": "",
        "bgcolor": "#0086be",
        "icon": "",
        "payload": "XRPBTC",
        "payloadType": "str",
        "topic": "",
        "x": 190,
        "y": 300,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "eda140e3.bc3b7",
        "type": "ui_slider",
        "z": "8cbdda14.b996b8",
        "name": "",
        "label": "",
        "group": "45d16510.12541c",
        "order": 9,
        "width": 0,
        "height": 0,
        "passthru": true,
        "topic": "",
        "min": "2",
        "max": "14",
        "step": 1,
        "x": 190,
        "y": 620,
        "wires": [
            [
                "9caeba9e.ffc1c8"
            ]
        ]
    },
    {
        "id": "dd3753cb.f4be2",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "set values for api",
        "func": "\nvar interval = \"1d\";\n\nswitch(msg.payload) {\n case 0:\n interval = \"1m\";\n break;\n case 1:\n interval = \"3m\";\n break;\n case 2:\n interval = \"5m\";\n break; \n case 3:\n interval = \"15m\";\n break;\n case 4:\n interval = \"1h\";\n break;\n case 5:\n interval = \"2h\";\n break;\n case 6:\n interval = \"4h\";\n break;\n case 7:\n interval = \"6h\";\n break;\n case 8:\n interval = \"8h\";\n break;\n case 9:\n interval = \"12h\";\n break;\n case 10:\n interval = \"1d\";\n break;\n case 11:\n interval = \"3d\";\n break;\n case 12:\n interval = \"1w\";\n break;\n case 13:\n interval = \"1M\";\n break;\n}\n\nflow.set(\"interval\", interval);\nmsg.payload = interval;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 510,
        "y": 620,
        "wires": [
            [
                "89cda091.9935a",
                "c5ee73a8.47dcd"
            ]
        ]
    },
    {
        "id": "9caeba9e.ffc1c8",
        "type": "delay",
        "z": "8cbdda14.b996b8",
        "name": "",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "3",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "x": 330,
        "y": 620,
        "wires": [
            [
                "dd3753cb.f4be2"
            ]
        ]
    },
    {
        "id": "52850f5b.f9227",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 9,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Monero",
        "color": "",
        "bgcolor": "#ff6600",
        "icon": "",
        "payload": "XMRBTC",
        "payloadType": "str",
        "topic": "",
        "x": 200,
        "y": 340,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "89cda091.9935a",
        "type": "ui_text",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "order": 8,
        "width": "0",
        "height": "0",
        "name": "",
        "label": "Chart time interval",
        "format": "{{msg.payload}}",
        "layout": "row-spread",
        "x": 730,
        "y": 620,
        "wires": []
    },
    {
        "id": "f1a0ec73.8f8c3",
        "type": "inject",
        "z": "8cbdda14.b996b8",
        "name": "initialize",
        "topic": "10",
        "payload": "10",
        "payloadType": "num",
        "repeat": "",
        "crontab": "",
        "once": true,
        "x": 200,
        "y": 580,
        "wires": [
            [
                "eda140e3.bc3b7"
            ]
        ]
    },
    {
        "id": "df2eaad5.6a4b88",
        "type": "ui_numeric",
        "z": "8cbdda14.b996b8",
        "name": "",
        "label": "# datapoints",
        "group": "45d16510.12541c",
        "order": 10,
        "width": 0,
        "height": 0,
        "passthru": true,
        "topic": "",
        "format": "{{value}}",
        "min": "1",
        "max": "500",
        "step": 1,
        "x": 210,
        "y": 520,
        "wires": [
            [
                "b9aae74d.dbe688"
            ]
        ]
    },
    {
        "id": "23d57d3.534a682",
        "type": "inject",
        "z": "8cbdda14.b996b8",
        "name": "initialize",
        "topic": "30",
        "payload": "30",
        "payloadType": "num",
        "repeat": "",
        "crontab": "",
        "once": true,
        "x": 200,
        "y": 480,
        "wires": [
            [
                "df2eaad5.6a4b88"
            ]
        ]
    },
    {
        "id": "73d1a156.c214d",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "set values for api",
        "func": "flow.set(\"limit\", parseInt(msg.payload));\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 570,
        "y": 520,
        "wires": [
            [
                "c5ee73a8.47dcd"
            ]
        ]
    },
    {
        "id": "c5ee73a8.47dcd",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "load chart options",
        "func": "msg.topic = flow.get(\"selectedTickerPair\");\nmsg.payload = {};\nmsg.payload.interval = flow.get(\"interval\");\nmsg.payload.limit = flow.get(\"limit\");\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 770,
        "y": 520,
        "wires": [
            [
                "1fca9467.e7501c"
            ]
        ]
    },
    {
        "id": "b9aae74d.dbe688",
        "type": "delay",
        "z": "8cbdda14.b996b8",
        "name": "",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "3",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "x": 370,
        "y": 520,
        "wires": [
            [
                "73d1a156.c214d"
            ]
        ]
    },
    {
        "id": "2c8481a.f4d337e",
        "type": "ui_toast",
        "z": "8cbdda14.b996b8",
        "position": "top right",
        "displayTime": "3",
        "highlight": "",
        "outputs": 0,
        "ok": "OK",
        "cancel": "",
        "topic": "Refreshing chart",
        "name": "",
        "x": 490,
        "y": 100,
        "wires": []
    },
    {
        "id": "5ff981d0.f7fb4",
        "type": "ui_template",
        "z": "8cbdda14.b996b8",
        "group": "45d16510.12541c",
        "name": "Logo display",
        "order": 1,
        "width": "4",
        "height": "3",
        "format": "<div ng-bind-html=\"msg.payload\"></div>",
        "storeOutMessages": true,
        "fwdInMessages": true,
        "templateScope": "local",
        "x": 910,
        "y": 340,
        "wires": [
            []
        ]
    },
    {
        "id": "c71a5eb7.0712d",
        "type": "function",
        "z": "8cbdda14.b996b8",
        "name": "load logo",
        "func": "var symbol = msg.topic.slice(0,-3);\nsymbol = symbol.toLowerCase();\n\nif (symbol === \"btcu\") { // USDT case\n symbol = \"btc\";\n}\n\nvar imgUrl = \"https://raw.githubusercontent.com/cjdowner/cryptocurrency-icons/master/128/color/\"+symbol+\".png\";\n\nmsg.payload = \"<img src='\"+imgUrl+\"'alt='Logo image not found' />\";\n\nreturn msg",
        "outputs": 1,
        "noerr": 0,
        "x": 760,
        "y": 340,
        "wires": [
            [
                "5ff981d0.f7fb4"
            ]
        ]
    },
    {
        "id": "ab114c85.0d99e",
        "type": "delay",
        "z": "8cbdda14.b996b8",
        "name": "",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "2",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "x": 470,
        "y": 200,
        "wires": [
            [
                "5eae3dbd.e743f4"
            ]
        ]
    },
    {
        "id": "c00f19be.27af98",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 9,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Stellar",
        "color": "",
        "bgcolor": "#99a2a6",
        "icon": "",
        "payload": "XLMBTC",
        "payloadType": "str",
        "topic": "",
        "x": 190,
        "y": 380,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "70f9e737.78b078",
        "type": "ui_button",
        "z": "8cbdda14.b996b8",
        "name": "",
        "group": "49b58639.d95708",
        "order": 9,
        "width": "2",
        "height": "1",
        "passthru": false,
        "label": "Zcash",
        "color": "",
        "bgcolor": "#e49b2e",
        "icon": "",
        "payload": "ZECBTC",
        "payloadType": "str",
        "topic": "",
        "x": 190,
        "y": 420,
        "wires": [
            [
                "a2e9524.1e63bb"
            ]
        ]
    },
    {
        "id": "e99f75a3.e130e8",
        "type": "delay",
        "z": "8cbdda14.b996b8",
        "name": "",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": true,
        "x": 470,
        "y": 680,
        "wires": [
            [
                "4cccdfc1.fc7ca"
            ]
        ]
    },
    {
        "id": "4f46e721.d31938",
        "type": "comment",
        "z": "8cbdda14.b996b8",
        "name": "文档：http://noderedguide.com/tutorial-advanced-dashboards-for-node-red-and-cryptocurrency/",
        "info": "",
        "x": 350,
        "y": 20,
        "wires": []
    },
    {
        "id": "40bfa87b.b15ac8",
        "type": "comment",
        "z": "8cbdda14.b996b8",
        "name": "Demo: https://sensetecnic.fred.sensetecnic.com/api/ui/#/0",
        "info": "",
        "x": 230,
        "y": 60,
        "wires": []
    },
    {
        "id": "45d16510.12541c",
        "type": "ui_group",
        "z": "",
        "name": "Numbers",
        "tab": "80f9f6fd.8c4bc8",
        "order": 1,
        "disp": false,
        "width": "5"
    },
    {
        "id": "49b58639.d95708",
        "type": "ui_group",
        "z": "",
        "name": "Chart",
        "tab": "80f9f6fd.8c4bc8",
        "order": 2,
        "disp": false,
        "width": "18"
    },
    {
        "id": "80f9f6fd.8c4bc8",
        "type": "ui_tab",
        "z": "",
        "name": "Binance Node Demo",
        "icon": "fa-bitcoin"
    }
]
```

# 5、学习资源



1. [Learning Node-RED 2.安装Node-RED - CSDN博客](https://blog.csdn.net/flx413/article/details/80740715)
2. [Go.IoT 社区](https://bb.goiot.cc/)
3. [Learning Node-RED 3.Node-RED的编程模型 - CSDN博客](https://blog.csdn.net/flx413/article/details/80963070)
4. [Learning Node-RED 4.安装dashboard模块 - CSDN博客](https://blog.csdn.net/flx413/article/details/80963397)
5. [node-red教程 7dashboard简介与输入型仪表板控件的使用 - CSDN博客](https://blog.csdn.net/geek_monkey/article/details/80755899)
6. [node-red教程7.3 常见的显示型仪表板控件应用 - CSDN博客](https://blog.csdn.net/geek_monkey/article/details/8075701)
7. [Node-red学习第8篇--关于模块dashboard中chart节点多数据统计显示的实现 - CSDN博客](https://blog.csdn.net/Enl0ve/article/details/80788047)
8. [node-red教程 5 函数节点 - CSDN博客](https://blog.csdn.net/geek_monkey/article/details/80751151)
9. [Go.IoT  一个在线使用和学习Red-Red的网站](https://goiot.cc/])

# 6、最佳实践

- 避免在一个流中使用太多节点。 如果节点数量太多，请尝试创建子流程设计可重用的节点/子流程
- 在函数节点内插入coNodemments以进一步理解函数功能
- 在settings.js中设置flowFilePretty：true，以便逐行格式化流而不是全部压缩，这样可以更容易地看到已更改的内容。
- 经常备份流。 这可以通过将流转储为json并存储在Git或任何其他版本控制系统中来完成
- 将事件记录到控制台以更好地了解发生的事件
- 一旦流程调试完毕，就应该断开/禁用调试节点
- 使用Web服务时，请确保重用现有节点，或者目标服务器能够处理异步请求
- 为节点分配适当的名称以识别输出消息的来源。 在调试时会很有帮助
- 尝试利用上下文变量跨流程共享数据