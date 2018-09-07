# export of my node-red controller for mysensors

[
    {
        "id": "bd3d1756.650818",
        "type": "switch",
        "z": "5a4c4744.8ddec8",
        "name": "switch message type",
        "property": "messageType",
        "rules": [
            {
                "t": "eq",
                "v": "0"
            },
            {
                "t": "eq",
                "v": "1"
            },
            {
                "t": "eq",
                "v": "2"
            },
            {
                "t": "eq",
                "v": "3"
            },
            {
                "t": "eq",
                "v": "4"
            }
        ],
        "checkall": "true",
        "outputs": 5,
        "x": 975.1666107177734,
        "y": 207.92859268188477,
        "wires": [
            [
                "39252489.0f922c"
            ],
            [
                "174d1ccc.f90c03"
            ],
            [
                "218a88c3.137418"
            ],
            [
                "aa52767e.4331e8"
            ],
            [
                "d8ecbbe9.30f6d8"
            ]
        ]
    },
    {
        "id": "39252489.0f922c",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "presentation",
        "func": "msg.subTypeString = context.global.MYS.SString(msg.subType);\n\nmsg.topic = context.global.MYS.TOPIC_PREFIX + '/' + msg.controller + \"/\" + msg.nodeId + \"/\" + msg.childSensorId + \"/\" + msg.subTypeString;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1202.6308670043945,
        "y": 138.65475463867188,
        "wires": [
            [
                "5ca80f9d.4f477",
                "6dcc37de.cb6b28"
            ]
        ]
    },
    {
        "id": "218a88c3.137418",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "req",
        "func": "msg.topic = \"req\";\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1183.214241027832,
        "y": 230.15476989746094,
        "wires": [
            [
                "6ba193be.a4d08c",
                "5ca80f9d.4f477"
            ]
        ]
    },
    {
        "id": "aa52767e.4331e8",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "internal",
        "func": "msg.subTypeString = context.global.MYS.IString(msg.subType);\n\nmsg.topic = context.global.MYS.TOPIC_PREFIX + '/' + msg.controller + \"/\" + msg.nodeId + \"/\" + msg.childSensorId + \"/\" + msg.subTypeString;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1194.214241027832,
        "y": 277.40476989746094,
        "wires": [
            [
                "5ca80f9d.4f477",
                "46acce36.a3a11",
                "e51532dd.551a1"
            ]
        ]
    },
    {
        "id": "d8ecbbe9.30f6d8",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "stream",
        "func": "msg.topic = \"stream\";\nreturn msg;",
        "outputs": 1,
        "x": 1194.214241027832,
        "y": 320.40476989746094,
        "wires": [
            []
        ]
    },
    {
        "id": "46acce36.a3a11",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "debug",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 1443.6071166992188,
        "y": 316.1190528869629,
        "wires": []
    },
    {
        "id": "174d1ccc.f90c03",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "set",
        "func": "msg.subTypeString = context.global.MYS.VString(msg.subType);\n\nmsg.topic = context.global.MYS.TOPIC_PREFIX + '/' + msg.controller + \"/\" + msg.nodeId + \"/\" + msg.childSensorId + \"/\" + msg.subTypeString;\n\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 1186.8808670043945,
        "y": 185.90476989746094,
        "wires": [
            [
                "5ca80f9d.4f477",
                "3a7a68eb.40ab58"
            ]
        ]
    },
    {
        "id": "d84d8c08.bb889",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 934.4880676269531,
        "y": 290.9523696899414,
        "wires": []
    },
    {
        "id": "594d880.6323578",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Split GW Message",
        "func": "\n    var tokens = msg.payload.split(\";\")\n    \n    msg.rawData = tokens;\n    if(tokens.length >= 6)\n    {\n        msg.nodeId = parseInt(tokens[0]);\n        msg.childSensorId = parseInt(tokens[1]);\n        msg.messageType = parseInt(tokens[2]);\n        msg.ack = parseInt(tokens[3]);\n        msg.subType = parseInt(tokens[4]);\n        msg.payload = tokens[5];\n        for (j=6; j<tokens.length; j++) \n            msg.payload = msg.payload + ';' + tokens[j];\n    }\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 735.6666259765625,
        "y": 205.66664123535156,
        "wires": [
            [
                "bd3d1756.650818",
                "d84d8c08.bb889",
                "43d02e76.605f"
            ]
        ]
    },
    {
        "id": "5ca80f9d.4f477",
        "type": "mqtt out",
        "z": "5a4c4744.8ddec8",
        "name": "Publish to MQTT",
        "topic": "",
        "qos": "",
        "retain": "",
        "broker": "26224260.2d8dde",
        "x": 1475.0594177246094,
        "y": 69.99999713897705,
        "wires": []
    },
    {
        "id": "e51532dd.551a1",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "hanlde internal messages",
        "func": "//msg.subTypeString = \"\";\n//msg.topic = \"internal respones\";\n\nif (msg.subTypeString == 'I_TIME')\n{\n    var payload = parseInt(new Date().getTime()/1000);\n\tvar command = context.global.MYS.T_INTERNAL; \n\tvar acknowledge = 0; // no ack\n\tvar type = context.global.MYS.I_TIME; // I_TIME\n\tmsg.payload = context.global.MYS.encode(msg.nodeId, msg.childSensorId, command, acknowledge, type, payload);\n    return msg;\n    \n}\nelse if  (msg.subTypeString == 'I_ID_REQUEST')\n{\n    msg.topic = \"I_ID_RESPONSE\";\n    msg.subTypeString = \"I_ID_RESPONSE\";\n    // 255;255;3;0;4;8 for ID 8\n    var command = context.global.MYS.T_INTERNAL;\n\tvar acknowledge = 0; // no ack\n\tvar type = context.global.MYS.I_ID_RESPONSE; // I_ID_RESPONSE\n\n\tif (context.global.mysnextid[msg.controller] === undefined) {\n\t    payload = context.global.MYS.MIN_NODEID;\n\t}    \n\telse\n\t{\n\t    payload = context.global.mysnextid[msg.controller];\n\t}\n\t\n    msg.payload = context.global.MYS.encode(msg.nodeId, msg.childSensorId, command, acknowledge, type, payload);\n    return msg; \n} \nelse if  (msg.subTypeString == 'I_CONFIG')\n{\n    var payload = 'M';\n\tvar command = context.global.MYS.T_INTERNAL; \n\tvar acknowledge = 0; // no ack\n\tvar type = context.global.MYS.I_CONFIG; // I_TIME\n\tmsg.payload = context.global.MYS.encode(msg.nodeId, msg.childSensorId, command, acknowledge, type, payload);\n    return msg;\n}\nelse if  (msg.subTypeString == 'I_INCLUSION_MODE')\n{\n    // not yet implemented\n}\n\nreturn;\n\n\n",
        "outputs": 1,
        "noerr": 0,
        "x": 1395.6308670043945,
        "y": 390.8214569091797,
        "wires": [
            [
                "46acce36.a3a11",
                "24eb1cdb.10f254"
            ]
        ]
    },
    {
        "id": "a65b0e2e.8854d",
        "type": "inject",
        "z": "5a4c4744.8ddec8",
        "name": "Startup",
        "topic": "",
        "payload": "",
        "payloadType": "str",
        "repeat": "",
        "crontab": "",
        "once": true,
        "x": 303.25,
        "y": 59.416656494140625,
        "wires": [
            [
                "96ab3fbd.49d1",
                "f8805251.aa54e"
            ]
        ]
    },
    {
        "id": "640a5b3f.69dde4",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "ToString",
        "func": "// sometimes we are receiving a several messages in one packet\n// we should spit them and emit one by one\nvar outputMsg = [];\n\nmsg.payload = msg.payload || '';\n\nmsg.payload = msg.payload.toString();\nvar parts = msg.payload.split(\"\\n\");\nfor (var id in parts) {\n    var payload = parts[id].replace(/[\\n\\r]/g, '');\n    if (payload.length > 0) {\n        outputMsg.push({ \n            payload: payload, \n            controller: msg.controller\n        });\n    }\n}\n\nreturn [outputMsg];",
        "outputs": 1,
        "noerr": 0,
        "x": 558.1666870117188,
        "y": 204.91668701171875,
        "wires": [
            [
                "594d880.6323578",
                "e27f5cae.ad2d8"
            ]
        ]
    },
    {
        "id": "96ab3fbd.49d1",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "MYS Initialize",
        "func": "function MySHelper() { \n\nMySHelper.prototype.TOPIC_PREFIX = \"MYS-NODERED\";\n\nMySHelper.prototype.MIN_NODEID = 10;\n\n\n// don't touch below :)\n    \nMySHelper.prototype.T_PRESENTATION                = 0;\nMySHelper.prototype.T_SET                         = 1;\nMySHelper.prototype.T_REQ                         = 2;\nMySHelper.prototype.T_INTERNAL                    = 3;\nMySHelper.prototype.T_STREAM                      = 4;\n\nMySHelper.prototype.I_BATTERY_LEVEL               = 0;\nMySHelper.prototype.I_TIME                        = 1;\nMySHelper.prototype.I_VERSION                     = 2;\nMySHelper.prototype.I_ID_REQUEST                  = 3;\nMySHelper.prototype.I_ID_RESPONSE                 = 4;\nMySHelper.prototype.I_INCLUSION_MODE              = 5;\nMySHelper.prototype.I_CONFIG                      = 6;\nMySHelper.prototype.I_PING                        = 7;\nMySHelper.prototype.I_PING_ACK                    = 8;\nMySHelper.prototype.I_LOG_MESSAGE                 = 9;\nMySHelper.prototype.I_CHILDREN                    = 10;\nMySHelper.prototype.I_SKETCH_NAME                 = 11;\nMySHelper.prototype.I_SKETCH_VERSION              = 12;\nMySHelper.prototype.I_REBOOT                      = 13;\nMySHelper.prototype.I_GATEWAY_READY               = 14;\nMySHelper.prototype.I_REQUEST_SIGNING             = 15;\nMySHelper.prototype.I_GET_NONCE                   = 16;\nMySHelper.prototype.I_GET_NONCE_RESPONSE          = 17;\nMySHelper.prototype.I_HEARTBEAT                   = 18;\nMySHelper.prototype.I_PRESENTATION                = 19;\n \nvar itype = {\nI_BATTERY_LEVEL:                    MySHelper.prototype.I_BATTERY_LEVEL,\nI_TIME:                             MySHelper.prototype.I_TIME,\nI_VERSION:                          MySHelper.prototype.I_VERSION,\nI_ID_REQUEST:                       MySHelper.prototype.I_ID_REQUEST,\nI_ID_RESPONSE:                      MySHelper.prototype.I_ID_RESPONSE,\nI_INCLUSION_MODE:                   MySHelper.prototype.I_INCLUSION_MODE,\nI_CONFIG:                           MySHelper.prototype.I_CONFIG,\nI_PING:                             MySHelper.prototype.I_PING,\nI_PING_ACK:                         MySHelper.prototype.I_PING_ACK,\nI_LOG_MESSAGE:                      MySHelper.prototype.I_LOG_MESSAGE,\nI_CHILDREN:                         MySHelper.prototype.I_CHILDREN,\nI_SKETCH_NAME:                      MySHelper.prototype.I_SKETCH_NAME,\nI_SKETCH_VERSION:                   MySHelper.prototype.I_SKETCH_VERSION,\nI_REBOOT:                           MySHelper.prototype.I_REBOOT,\nI_GATEWAY_READY:                    MySHelper.prototype.I_GATEWAY_READY,\nI_REQUEST_SIGNING:                  MySHelper.prototype.I_REQUEST_SIGNING,\nI_GET_NONCE:                        MySHelper.prototype.I_GET_NONCE,\nI_GET_NONCE_RESPONSE:               MySHelper.prototype.I_GET_NONCE_RESPONSE,\nI_HEARTBEAT:                        MySHelper.prototype.I_HEARTBEAT,\nI_PRESENTATION:                     MySHelper.prototype.I_PRESENTATION\n};\n\nMySHelper.prototype.IString = function(I_SUBTYPE) {\n    for (var prop in itype ) \n    if( itype[ prop ] === parseInt(I_SUBTYPE) )\n        return prop;\n};\n\nMySHelper.prototype.V_TEMP                = 0;\nMySHelper.prototype.V_HUM                 = 1;\nMySHelper.prototype.V_LIGHT               = 2;\nMySHelper.prototype.V_STATUS              = 2;\nMySHelper.prototype.V_DIMMER              = 3;\nMySHelper.prototype.V_PRESSURE            = 4;\nMySHelper.prototype.V_FORECAST            = 5;\nMySHelper.prototype.V_RAIN                = 6;\nMySHelper.prototype.V_RAINRATE            = 7;\nMySHelper.prototype.V_WIND                = 8;\nMySHelper.prototype.V_GUST                = 9;\nMySHelper.prototype.V_DIRECTION           = 10;\nMySHelper.prototype.V_UV                  = 11;\nMySHelper.prototype.V_WEIGHT              = 12;\nMySHelper.prototype.V_DISTANCE            = 13;\nMySHelper.prototype.V_IMPEDANCE           = 14;\nMySHelper.prototype.V_ARMED               = 15;\nMySHelper.prototype.V_TRIPPED             = 16;\nMySHelper.prototype.V_WATT                = 17;\nMySHelper.prototype.V_KWH                 = 18;\nMySHelper.prototype.V_SCENE_ON            = 19;\nMySHelper.prototype.V_SCENE_OFF           = 20;\nMySHelper.prototype.V_HEATER              = 21;\nMySHelper.prototype.V_HEATER_SW           = 22;\nMySHelper.prototype.V_LIGHT_LEVEL         = 23;\nMySHelper.prototype.V_VAR1                = 24;\nMySHelper.prototype.V_VAR2                = 25;\nMySHelper.prototype.V_VAR3                = 26;\nMySHelper.prototype.V_VAR4                = 27;\nMySHelper.prototype.V_VAR5                = 28;\nMySHelper.prototype.V_UP                  = 29;\nMySHelper.prototype.V_DOWN                = 30;\nMySHelper.prototype.V_STOP                = 31;\nMySHelper.prototype.V_IR_SEND             = 32;\nMySHelper.prototype.V_IR_RECEIVE          = 33;\nMySHelper.prototype.V_FLOW                = 34;\nMySHelper.prototype.V_VOLUME              = 35;\nMySHelper.prototype.V_LOCK_STATUS         = 36;\nMySHelper.prototype.V_LEVEL               = 37; \nMySHelper.prototype.V_VOLTAGE             = 38; \nMySHelper.prototype.V_CURRENT             = 39; \nMySHelper.prototype.V_RGB                 = 40; \nMySHelper.prototype.V_RGBW                = 41;\nMySHelper.prototype.V_ID                  = 42;\nMySHelper.prototype.V_UNIT_PREFIX         = 43;\nMySHelper.prototype.V_HVAC_SETPOINT_COOL  = 44;\nMySHelper.prototype.V_HVAC_SETPOINT_HEAT  = 45;\nMySHelper.prototype.V_HVAC_FLOW_MODE      = 46;\nMySHelper.prototype.V_TEXT                = 47;\nMySHelper.prototype.V_CUSTOM\t\t\t  = 48;\t//!< Custom messages used for controller/inter node specific commands, preferably using S_CUSTOM device type.\nMySHelper.prototype.V_POSITION\t\t  \t  = 49;\t//!< GPS position and altitude. Payload: latitude;longitude;altitude(m). E.g. \"55.722526;13.017972;18\"\nMySHelper.prototype.V_IR_RECORD\t\t\t  = 50;\t//!< Record IR codes S_IR for playback\nMySHelper.prototype.V_PH\t\t\t\t  = 51;\t//!< S_WATER_QUALITY, water PH\nMySHelper.prototype.V_ORP\t\t\t\t  = 52;\t//!< S_WATER_QUALITY, water ORP : redox potential in mV\nMySHelper.prototype.V_EC\t\t\t\t  = 53;\t//!< S_WATER_QUALITY, water electric conductivity μS/cm (microSiemens/cm)\nMySHelper.prototype.V_VAR\t\t\t\t  = 54;\t//!< S_POWER, Reactive power: volt-ampere reactive (var)\nMySHelper.prototype.V_VA\t\t\t\t  = 55;\t//!< S_POWER, Apparent power: volt-ampere (VA)\nMySHelper.prototype.V_POWER_FACTOR\t\t  = 56;\t//!< S_POWER, Ratio of real power to apparent power: floating point value in the range [-1,..,1]\n\n\nvar vtype = {\nV_TEMP:              MySHelper.prototype.V_TEMP,\nV_HUM:               MySHelper.prototype.V_HUM,\nV_STATUS:            MySHelper.prototype.V_STATUS,\nV_LIGHT:             MySHelper.prototype.V_LIGHT,\nV_DIMMER:            MySHelper.prototype.V_DIMMER,\nV_PRESSURE:          MySHelper.prototype.V_PRESSURE,\nV_FORECAST:          MySHelper.prototype.V_FORECAST,\nV_RAIN:              MySHelper.prototype.V_RAIN,\nV_RAINRATE:          MySHelper.prototype.V_RAINRATE,\nV_WIND:              MySHelper.prototype.V_WIND,\nV_GUST:              MySHelper.prototype.V_GUST,\nV_DIRECTION:         MySHelper.prototype.V_DIRECTION,\nV_UV:                MySHelper.prototype.V_UV,\nV_WEIGHT:            MySHelper.prototype.V_WEIGHT,\nV_DISTANCE:          MySHelper.prototype.V_DISTANCE,\nV_IMPEDANCE:         MySHelper.prototype.V_IMPEDANCE,\nV_ARMED:             MySHelper.prototype.V_ARMED,\nV_TRIPPED:           MySHelper.prototype.V_TRIPPED,\nV_WATT:              MySHelper.prototype.V_WATT,\nV_KWH:               MySHelper.prototype.V_KWH,\nV_SCENE_ON:          MySHelper.prototype.V_SCENE_ON,\nV_SCENE_OFF:         MySHelper.prototype.V_SCENE_OFF,\nV_HEATER:            MySHelper.prototype.V_HEATER,\nV_HEATER_SW:         MySHelper.prototype.V_HEATER_SW,\nV_LIGHT_LEVEL:       MySHelper.prototype.V_LIGHT_LEVEL,\nV_VAR1:              MySHelper.prototype.V_VAR1,\nV_VAR2:              MySHelper.prototype.V_VAR2,\nV_VAR3:              MySHelper.prototype.V_VAR3,\nV_VAR4:              MySHelper.prototype.V_VAR4,\nV_VAR5:              MySHelper.prototype.V_VAR5,\nV_UP:                MySHelper.prototype.V_UP,\nV_DOWN:              MySHelper.prototype.V_DOWN,\nV_STOP:              MySHelper.prototype.V_STOP,\nV_IR_SEND:           MySHelper.prototype.V_IR_SEND,\nV_IR_RECEIVE:        MySHelper.prototype.V_IR_RECEIVE,\nV_FLOW:              MySHelper.prototype.V_FLOW,\nV_VOLUME:            MySHelper.prototype.V_VOLUME,\nV_LOCK_STATUS:       MySHelper.prototype.V_LOCK_STATUS,\nV_LEVEL:             MySHelper.prototype.V_LEVEL,\nV_VOLTAGE:           MySHelper.prototype.V_VOLTAGE, \nV_CURRENT:           MySHelper.prototype.V_CURRENT,\nV_RGB:               MySHelper.prototype.V_RGB,\nV_RGBW:              MySHelper.prototype.V_RGBW,\nV_ID:                MySHelper.prototype.V_ID,\nV_UNIT_PREFIX:       MySHelper.prototype.V_UNIT_PREFIX,\nV_HVAC_SETPOINT_COOL:MySHelper.prototype.V_HVAC_SETPOINT_COOL,\nV_HVAC_SETPOINT_HEAT:MySHelper.prototype.V_HVAC_SETPOINT_HEAT,\nV_HVAC_FLOW_MODE:    MySHelper.prototype.V_HVAC_FLOW_MODE,\nV_TEXT:              MySHelper.prototype.V_TEXT,\nV_CUSTOM:            MySHelper.prototype.V_CUSTOM,\t//!< Custom messages used for controller/inter node specific commands, preferably using S_CUSTOM device type.\nV_POSITION:\t\t\t MySHelper.prototype.V_POSITION,\t//!< GPS position and altitude. Payload: latitude;longitude;altitude(m). E.g. \"55.722526;13.017972;18\"\nV_IR_RECORD:\t\t MySHelper.prototype.V_IR_RECORD,\t//!< Record IR codes S_IR for playback\nV_PH:\t\t\t\t MySHelper.prototype.V_PH,\t//!< S_WATER_QUALITY, water PH\nV_ORP:\t\t\t\t MySHelper.prototype.V_ORP,\t//!< S_WATER_QUALITY, water ORP : redox potential in mV\nV_EC:\t\t\t\t MySHelper.prototype.V_EC,\t//!< S_WATER_QUALITY, water electric conductivity μS/cm (microSiemens/cm)\nV_VAR:\t\t\t\t MySHelper.prototype.V_VAR,\t//!< S_POWER, Reactive power: volt-ampere reactive (var)\nV_VA:\t\t\t\t MySHelper.prototype.V_VA,\t//!< S_POWER, Apparent power: volt-ampere (VA)\nV_POWER_FACTOR:\t\t MySHelper.prototype.V_POWER_FACTOR\t//!< S_POWER, Ratio of real power to apparent power: floating point value in the range [-1,..,1]\n};\n\n\n\nMySHelper.prototype.VString = function(V_SUBTYPE) {\n    for (var prop in vtype ) \n    if( vtype[ prop ] === parseInt(V_SUBTYPE) )\n        return prop;\n};\n    \nMySHelper.prototype.VNum = function(V_SUBTYPE) {\n    sRet = vtype [V_SUBTYPE];\n    if (sRet === undefined)\n        sRet = V_SUBTYPE;\n    return sRet;\n}\n\nMySHelper.prototype.S_DOOR = 0; // Door sensor; V_TRIPPED; V_ARMED\nMySHelper.prototype.S_MOTION = 1;  // Motion sensor; V_TRIPPED; V_ARMED \nMySHelper.prototype.S_SMOKE = 2;  // Smoke sensor; V_TRIPPED; V_ARMED\nMySHelper.prototype.S_LIGHT = 3; // Binary light or relay; V_STATUS (or V_LIGHT); V_WATT\nMySHelper.prototype.S_BINARY = 3; // Binary light or relay; V_STATUS (or V_LIGHT); V_WATT (same as MySHelper.prototype.S_LIGHT)\nMySHelper.prototype.S_DIMMER = 4; // Dimmable light or fan device; V_STATUS (on/off); V_DIMMER (dimmer level 0-100); V_WATT\nMySHelper.prototype.S_COVER = 5; // Blinds or window cover; V_UP; V_DOWN; V_STOP; V_DIMMER (open/close to a percentage)\nMySHelper.prototype.S_TEMP = 6; // Temperature sensor; V_TEMP\nMySHelper.prototype.S_HUM = 7; // Humidity sensor; V_HUM\nMySHelper.prototype.S_BARO = 8; // Barometer sensor; V_PRESSURE; V_FORECAST\nMySHelper.prototype.S_WIND = 9; // Wind sensor; V_WIND; V_GUST\nMySHelper.prototype.S_RAIN = 10; // Rain sensor; V_RAIN; V_RAINRATE\nMySHelper.prototype.S_UV = 11; // Uv sensor; V_UV\nMySHelper.prototype.S_WEIGHT = 12; // Personal scale sensor; V_WEIGHT; V_IMPEDANCE\nMySHelper.prototype.S_POWER = 13; // Power meter; V_WATT; V_KWH\nMySHelper.prototype.S_HEATER = 14; // Header device; V_HVAC_SETPOINT_HEAT; V_HVAC_FLOW_STATE; V_TEMP\nMySHelper.prototype.S_DISTANCE = 15; // Distance sensor; V_DISTANCE\nMySHelper.prototype.S_LIGHT_LEVEL = 16; // Light level sensor; V_LIGHT_LEVEL (uncalibrated in percentage);  V_LEVEL (light level in lux)\nMySHelper.prototype.S_ARDUINO_NODE = 17 ; // Used (internally) for presenting a non-repeating Arduino node\nMySHelper.prototype.S_ARDUINO_REPEATER_NODE = 18; // Used (internally) for presenting a repeating Arduino node \nMySHelper.prototype.S_LOCK = 19; // Lock device; V_LOCK_STATUS\nMySHelper.prototype.S_IR = 20; // Ir device; V_IR_SEND; V_IR_RECEIVE\nMySHelper.prototype.S_WATER = 21; // Water meter; V_FLOW; V_VOLUME\nMySHelper.prototype.S_AIR_QUALITY = 22; // Air quality sensor; V_LEVEL\nMySHelper.prototype.S_CUSTOM = 23; // Custom sensor \nMySHelper.prototype.S_DUST = 24; // Dust sensor; V_LEVEL\nMySHelper.prototype.S_SCENE_CONTROLLER = 25; // Scene controller device; V_SCENE_ON; V_SCENE_OFF. \nMySHelper.prototype.S_RGB_LIGHT = 26; // RGB light. Send color component data using V_RGB. Also supports V_WATT \nMySHelper.prototype.S_RGBW_LIGHT = 27; // RGB light with an additional White component. Send data using V_RGBW. Also supports V_WATT\nMySHelper.prototype.S_COLOR_SENSOR = 28;  // Color sensor; send color information using V_RGB\nMySHelper.prototype.S_HVAC = 29; // Thermostat/HVAC device. V_HVAC_SETPOINT_HEAT; V_HVAC_SETPOINT_COLD; V_HVAC_FLOW_STATE; V_HVAC_FLOW_MODE; V_TEMP\nMySHelper.prototype.S_MULTIMETER = 30; // Multimeter device; V_VOLTAGE; V_CURRENT; V_IMPEDANCE \nMySHelper.prototype.S_SPRINKLER  = 31;  // Sprinkler; V_STATUS (turn on/off); V_TRIPPED (if fire detecting device)\nMySHelper.prototype.S_WATER_LEAK = 32; // Water leak sensor; V_TRIPPED; V_ARMED\nMySHelper.prototype.S_SOUND = 33; // Sound sensor; V_TRIPPED; V_ARMED; V_LEVEL (sound level in dB)\nMySHelper.prototype.S_VIBRATION = 34; // Vibration sensor; V_TRIPPED; V_ARMED; V_LEVEL (vibration in Hz)\nMySHelper.prototype.S_MOISTURE = 35; // Moisture sensor; V_TRIPPED; V_ARMED; V_LEVEL (water content or moisture in percentage?) \nMySHelper.prototype.S_INFO = 36; // LCD text device / Simple information device on controller; V_TEXT\nMySHelper.prototype.S_GAS = 37; // Gas meter; V_FLOW; V_VOLUME\nMySHelper.prototype.S_GPS = 38; // GPS Sensor; V_POSITION\nMySHelper.prototype.S_WATER_QUALITY\t= 39;\t//!< V_TEMP, V_PH, V_ORP, V_EC, V_STATUS \n\nvar stype = {\nS_DOOR:                 MySHelper.prototype.S_DOOR, // Door sensor; V_TRIPPED; V_ARMED\nS_MOTION:               MySHelper.prototype.S_MOTION,  // Motion sensor; V_TRIPPED; V_ARMED \nS_SMOKE:                MySHelper.prototype.S_SMOKE,  // Smoke sensor; V_TRIPPED; V_ARMED\nS_LIGHT:                MySHelper.prototype.S_LIGHT, // Binary light or relay; V_STATUS (or V_LIGHT); V_WATT\nS_BINARY:               MySHelper.prototype.S_BINARY, // Binary light or relay; V_STATUS (or V_LIGHT); V_WATT (same as MySHelper.prototype.S_LIGHT)\nS_DIMMER:               MySHelper.prototype.S_DIMMER, // Dimmable light or fan device; V_STATUS (on/off); V_DIMMER (dimmer level 0-100); V_WATT\nS_COVER:                MySHelper.prototype.S_COVER, // Blinds or window cover; V_UP; V_DOWN; V_STOP; V_DIMMER (open/close to a percentage)\nS_TEMP:                 MySHelper.prototype.S_TEMP, // Temperature sensor; V_TEMP\nS_HUM:                  MySHelper.prototype.S_HUM, // Humidity sensor; V_HUM\nS_BARO:                 MySHelper.prototype.S_BARO, // Barometer sensor; V_PRESSURE; V_FORECAST\nS_WIND:                 MySHelper.prototype.S_WIND, // Wind sensor; V_WIND; V_GUST\nS_RAIN:                 MySHelper.prototype.S_RAIN, // Rain sensor; V_RAIN; V_RAINRATE\nS_UV:                   MySHelper.prototype.S_UV, // Uv sensor; V_UV\nS_WEIGHT:               MySHelper.prototype.S_WEIGHT, // Personal scale sensor; V_WEIGHT; V_IMPEDANCE\nS_POWER:                MySHelper.prototype.S_POWER, // Power meter; V_WATT; V_KWH\nS_HEATER:               MySHelper.prototype.S_HEATER, // Header device; V_HVAC_SETPOINT_HEAT; V_HVAC_FLOW_STATE; V_TEMP\nS_DISTANCE:             MySHelper.prototype.S_DISTANCE, // Distance sensor; V_DISTANCE\nS_LIGHT_LEVEL:          MySHelper.prototype.S_LIGHT_LEVEL, // Light level sensor; V_LIGHT_LEVEL (uncalibrated in percentage);  V_LEVEL (light level in lux)\nS_ARDUINO_NODE:         MySHelper.prototype.S_ARDUINO_NODE, // Used (internally) for presenting a non-repeating Arduino node\nS_ARDUINO_REPEATER_NODE:MySHelper.prototype.S_ARDUINO_REPEATER_NODE, // Used (internally) for presenting a repeating Arduino node \nS_LOCK:                 MySHelper.prototype.S_LOCK, // Lock device; V_LOCK_STATUS\nS_IR:                   MySHelper.prototype.S_IR, // Ir device; V_IR_SEND; V_IR_RECEIVE\nS_WATER:                MySHelper.prototype.S_WATER, // Water meter; V_FLOW; V_VOLUME\nS_AIR_QUALITY:          MySHelper.prototype.S_AIR_QUALITY, // Air quality sensor; V_LEVEL\nS_CUSTOM:               MySHelper.prototype.S_CUSTOM, // Custom sensor \nS_DUST:                 MySHelper.prototype.S_DUST, // Dust sensor; V_LEVEL\nS_SCENE_CONTROLLER:     MySHelper.prototype.S_SCENE_CONTROLLER, // Scene controller device; V_SCENE_ON; V_SCENE_OFF. \nS_RGB_LIGHT:            MySHelper.prototype.S_RGB_LIGHT, // RGB light. Send color component data using V_RGB. Also supports V_WATT \nS_RGBW_LIGHT:           MySHelper.prototype.S_RGBW_LIGHT, // RGB light with an additional White component. Send data using V_RGBW. Also supports V_WATT\nS_COLOR_SENSOR:         MySHelper.prototype.S_COLOR_SENSOR,  // Color sensor; send color information using V_RGB\nS_HVAC:                 MySHelper.prototype.S_HVAC, // Thermostat/HVAC device. V_HVAC_SETPOINT_HEAT; V_HVAC_SETPOINT_COLD; V_HVAC_FLOW_STATE; V_HVAC_FLOW_MODE; V_TEMP\nS_MULTIMETER:           MySHelper.prototype.S_MULTIMETER, // Multimeter device; V_VOLTAGE; V_CURRENT; V_IMPEDANCE \nS_SPRINKLER:            MySHelper.prototype.S_SPRINKLER,  // Sprinkler; V_STATUS (turn on/off); V_TRIPPED (if fire detecting device)\nS_WATER_LEAK:           MySHelper.prototype.S_WATER_LEAK, // Water leak sensor; V_TRIPPED; V_ARMED\nS_SOUND:                MySHelper.prototype.S_SOUND, // Sound sensor; V_TRIPPED; V_ARMED; V_LEVEL (sound level in dB)\nS_VIBRATION:            MySHelper.prototype.S_VIBRATION, // Vibration sensor; V_TRIPPED; V_ARMED; V_LEVEL (vibration in Hz)\nS_MOISTURE:             MySHelper.prototype.S_MOISTURE, // Moisture sensor; V_TRIPPED; V_ARMED; V_LEVEL (water content or moisture in percentage?) \nS_INFO:                 MySHelper.prototype.S_INFO, // LCD text device / Simple information device on controller; V_TEXT\nS_GAS:                  MySHelper.prototype.S_GAS, // Gas meter; V_FLOW; V_VOLUME\nS_GPS:                  MySHelper.prototype.S_GPS, // GPS Sensor; V_POSITION\nS_WATER_QUALITY:        MySHelper.prototype.S_WATER_QUALITY // GPS Sensor; V_POSITION\n};\n\nMySHelper.prototype.SString = function(S_SUBTYPE) {\n    for (var prop in stype ) \n    if( stype[ prop ] === parseInt(S_SUBTYPE) )\n        return prop;\n};    \n    \n MySHelper.prototype.encode = function(destination, sensor, command, acknowledge, type, payload) {\n\tvar msg = destination.toString(10) + \";\" + sensor.toString(10) + \";\" + command.toString(10) + \";\" + acknowledge.toString(10) + \";\" + type.toString(10) + \";\";\n\tif (command == 4) {\n\t\tfor (var i = 0; i < payload.length; i++) {\n\t\t\tif (payload[i] < 16)\n\t\t\t\tmsg += \"0\";\n\t\t\tmsg += payload[i].toString(16);\n\t\t}\n\t} else {\n\t\tmsg += payload;\n\t}\n\tmsg += '\\n';\n\treturn msg.toString();\n}\n}\n\n\nvar MySHelperObj = new MySHelper();\n\ncontext.global.MYS = MySHelperObj;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 477.25,
        "y": 58,
        "wires": [
            []
        ]
    },
    {
        "id": "53e5263d.054398",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Controller 1",
        "func": "msg.controller = 1\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 397,
        "y": 201.49999523162842,
        "wires": [
            [
                "640a5b3f.69dde4",
                "761df290.084aac"
            ]
        ]
    },
    {
        "id": "24eb1cdb.10f254",
        "type": "switch",
        "z": "5a4c4744.8ddec8",
        "name": "route to controller n",
        "property": "controller",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "1",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "2",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "3",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "4",
                "vt": "str"
            }
        ],
        "checkall": "false",
        "outputs": 4,
        "x": 771.4286193847656,
        "y": 385.8214063644409,
        "wires": [
            [
                "2831c5d.55e7e3a",
                "19b0c05d.0b196"
            ],
            [
                "3e2aa0d0.ee8c8"
            ],
            [],
            []
        ]
    },
    {
        "id": "f8805251.aa54e",
        "type": "file in",
        "z": "5a4c4744.8ddec8",
        "name": "read store for next ids",
        "filename": "/data/mysids.dump",
        "format": "utf8",
        "x": 503,
        "y": 98,
        "wires": [
            [
                "87cb266.2f83fd8"
            ]
        ]
    },
    {
        "id": "87cb266.2f83fd8",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "store to context.global.mysnextid",
        "func": "if (msg.payload === undefined) {\n          var mysnextid = {};\n\n        obj = mysnextid;  \n}\nelse \n{\ntry{\n        obj = JSON.parse(msg.payload);\n    }\n    catch(e){\n        \n        var mysnextid = {};\n\n        obj = mysnextid;\n        \n    }\n}    \n\ncontext.global.mysnextid = obj;\n\nmsg.payload = context.global.mysnextid;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 786,
        "y": 59,
        "wires": [
            []
        ]
    },
    {
        "id": "85756d23.adf1f",
        "type": "file",
        "z": "5a4c4744.8ddec8",
        "name": "dump mysids",
        "filename": "/data/mysids.dump",
        "appendNewline": true,
        "createDir": false,
        "overwriteFile": "true",
        "x": 1196.5713195800781,
        "y": 20,
        "wires": []
    },
    {
        "id": "43d02e76.605f",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "handle nextids",
        "func": "// increase nextid, if nodeid >= nextid \n// node.log(\"Handling next ids\");\n// node.log(\"nodeId: \" + msg.nodeId);\n\nbChanged = false;\n\nif (msg.nodeId === 0) return;\n\nif (msg.nodeId < 255)\n{\n    // node.log(\"Test if next node-id must be set\");\n    if (context.global.mysnextid[msg.controller] === undefined) {\n        node.log(\"node-id not initialized yet\");\n        context.global.mysnextid[msg.controller] = msg.nodeId+1;\n        if (context.global.mysnextid[msg.controller] < context.global.MYS.MIN_NODEID)\n            context.global.mysnextid[msg.controller] = context.global.MYS.MIN_NODEID;\n        bChanged = true;\n    }\n    \n    // node.log(\"Nexid stored \" + parseInt(context.global.mysnextid[msg.controller]));\n    \n    if (msg.nodeId >= parseInt(context.global.mysnextid[msg.controller])) {\n        node.log(\"node-id >= next -> increase\");\n        // convert to int\n        // context.global.mysnextid[msg.controller] = parseInt(context.global.mysnextid[msg.controller]);\n        context.global.mysnextid[msg.controller] = msg.nodeId+1; \n        if (context.global.mysnextid[msg.controller] < context.global.MYS.MIN_NODEID)\n            context.global.mysnextid[msg.controller] = context.global.MYS.MIN_NODEID;\n        bChanged = true;\n    }\n\n    if (bChanged) {\n        node.log(\"next id must be stored\");\n        msg.payload = JSON.stringify(context.global.mysnextid);   \n        return msg;\n    }\n}\n\n\n",
        "outputs": 1,
        "noerr": 0,
        "x": 957.714241027832,
        "y": 128.71430015563965,
        "wires": [
            [
                "85756d23.adf1f",
                "a2fc7433.8e1ea8"
            ]
        ]
    },
    {
        "id": "a2fc7433.8e1ea8",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "false",
        "x": 1418.9999771118164,
        "y": 20,
        "wires": []
    },
    {
        "id": "6dcc37de.cb6b28",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "debug",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 1440.8571166992188,
        "y": 127.11905288696289,
        "wires": []
    },
    {
        "id": "3a7a68eb.40ab58",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "debug",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 1440.8571166992188,
        "y": 194.1190528869629,
        "wires": []
    },
    {
        "id": "6ba193be.a4d08c",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "debug",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 1450.8571166992188,
        "y": 267.1190528869629,
        "wires": []
    },
    {
        "id": "c3e28b1c.294e18",
        "type": "mqtt in",
        "z": "5a4c4744.8ddec8",
        "name": "From Outside",
        "topic": "MYS-NODERED/+/+/+/+/SET",
        "qos": "2",
        "broker": "26224260.2d8dde",
        "x": 85,
        "y": 429.33334398269653,
        "wires": [
            [
                "38bc1b4c.f2b8b4"
            ]
        ]
    },
    {
        "id": "623d45f4.17477c",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "Dump from MQTT",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 516.9285583496094,
        "y": 497.71428966522217,
        "wires": []
    },
    {
        "id": "38bc1b4c.f2b8b4",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Parse",
        "func": "// msg.topic = context.global.MYS.TOPIC_PREFIX + '/' + msg.controller + \"/\" + msg.nodeId + \"/\" + msg.childSensorId + \"/\" + msg.subTypeString;\n\n\n    var tokens = msg.topic.split(\"/\");\n    \n    msg.rawData = tokens;\n    if(tokens.length >= 5)\n    {\n        msg.controller = parseInt(tokens[1]);\n        msg.nodeId = parseInt(tokens[2]);\n        msg.childSensorId = parseInt(tokens[3]);\n        msg.subTypeString = tokens[4];\n        msg.subType = context.global.MYS.VNum(msg.subTypeString);\n        msg.command = 1; // SET\n        msg.acknowledge = 0; // no ack as default\n    }\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 230.5,
        "y": 535.9999952316284,
        "wires": [
            [
                "623d45f4.17477c",
                "8ab37a92.93ffb8"
            ]
        ]
    },
    {
        "id": "8ab37a92.93ffb8",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Send to Gateway",
        "func": "msg.payload = context.global.MYS.encode(msg.nodeId, msg.childSensorId, msg.command, msg.acknowledge, msg.subType, msg.payload);\n\n//msg.payload = msg.nodeId + ' ' + msg.childSensorId + ' ' + msg.command;\n//msg.childSensorId, msg.command, msg.acknowledge, msg.subType, msg.payload);\n\n    \nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 522.5000305175781,
        "y": 542.571436882019,
        "wires": [
            [
                "2831c5d.55e7e3a",
                "24eb1cdb.10f254"
            ]
        ]
    },
    {
        "id": "2831c5d.55e7e3a",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "Serial",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 864.5,
        "y": 571,
        "wires": []
    },
    {
        "id": "294f68a8.ccf0a8",
        "type": "inject",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "topic": "Test",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 93.5,
        "y": 626.9999952316284,
        "wires": [
            [
                "aa4cc70c.347d68"
            ]
        ]
    },
    {
        "id": "aa4cc70c.347d68",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Relay On",
        "func": "msg.controller = 2;\nmsg.nodeId = 12;\nmsg.childSensorId = 3;\nmsg.subTypeString = 'V_LIGHT';\nmsg.subType = context.global.MYS.VNum(msg.subTypeString);\nmsg.command = 1; // SET\nmsg.acknowledge = 0; // no ack as default\nmsg.payload = 1;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 252.5,
        "y": 627.9999952316284,
        "wires": [
            [
                "8ab37a92.93ffb8"
            ]
        ]
    },
    {
        "id": "3cf989b1.a327e6",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Relay Off",
        "func": "msg.controller = 2;\nmsg.nodeId = 12;\nmsg.childSensorId = 3;\nmsg.subTypeString = 'V_LIGHT';\nmsg.subType = context.global.MYS.VNum(msg.subTypeString);\nmsg.command = 1; // SET\nmsg.acknowledge = 0; // no ack as default\nmsg.payload = 0;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 246,
        "y": 666.9999952316284,
        "wires": [
            [
                "8ab37a92.93ffb8"
            ]
        ]
    },
    {
        "id": "95d5b00f.7ec2a",
        "type": "inject",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "topic": "Test",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 93,
        "y": 669.9999952316284,
        "wires": [
            [
                "3cf989b1.a327e6"
            ]
        ]
    },
    {
        "id": "9e647788.661b98",
        "type": "inject",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "topic": "Test",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 94,
        "y": 708.9999952316284,
        "wires": [
            [
                "45d8eb97.71e174"
            ]
        ]
    },
    {
        "id": "45d8eb97.71e174",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Relay 2 On",
        "func": "msg.controller = 2;\nmsg.nodeId = 12;\nmsg.childSensorId = 4;\nmsg.subTypeString = 'V_LIGHT';\nmsg.subType = context.global.MYS.VNum(msg.subTypeString);\nmsg.command = 1; // SET\nmsg.acknowledge = 0; // no ack as default\nmsg.payload = 1;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 248,
        "y": 714.9999952316284,
        "wires": [
            [
                "8ab37a92.93ffb8"
            ]
        ]
    },
    {
        "id": "855f2f4b.02944",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Relay 2 Off",
        "func": "msg.controller = 2;\nmsg.nodeId = 12;\nmsg.childSensorId = 4;\nmsg.subTypeString = 'V_LIGHT';\nmsg.subType = context.global.MYS.VNum(msg.subTypeString);\nmsg.command = 1; // SET\nmsg.acknowledge = 0; // no ack as default\nmsg.payload = 0;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 253.5,
        "y": 753.9999952316284,
        "wires": [
            [
                "8ab37a92.93ffb8"
            ]
        ]
    },
    {
        "id": "3e9b6406.67398c",
        "type": "inject",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "topic": "Test",
        "payload": "",
        "payloadType": "date",
        "repeat": "",
        "crontab": "",
        "once": false,
        "x": 88.5,
        "y": 747.9999952316284,
        "wires": [
            [
                "855f2f4b.02944"
            ]
        ]
    },
    {
        "id": "e27f5cae.ad2d8",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 565,
        "y": 144.99999523162842,
        "wires": []
    },
    {
        "id": "3d4f7d29.b1de32",
        "type": "comment",
        "z": "5a4c4744.8ddec8",
        "name": "Testbed",
        "info": "",
        "x": 60,
        "y": 586.9999904632568,
        "wires": []
    },
    {
        "id": "eb9527d1.0a4bf8",
        "type": "function",
        "z": "5a4c4744.8ddec8",
        "name": "Set Controller 2",
        "func": "msg.controller = 2\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 411.7142639160156,
        "y": 293.9999952316284,
        "wires": [
            [
                "640a5b3f.69dde4",
                "761df290.084aac"
            ]
        ]
    },
    {
        "id": "761df290.084aac",
        "type": "debug",
        "z": "5a4c4744.8ddec8",
        "name": "",
        "active": false,
        "console": "false",
        "complete": "true",
        "x": 603.7142639160156,
        "y": 266.9999952316284,
        "wires": []
    },
    {
        "id": "1f9eaedf.eb6af1",
        "type": "comment",
        "z": "5a4c4744.8ddec8",
        "name": "Backup Mega",
        "info": "[\n    {\n        \"id\": \"ea210f89.d12a7\",\n        \"type\": \"serial in\",\n        \"z\": \"4be4e248.b41b1c\",\n        \"name\": \"MySensors Gateway Mega2560 Read\",\n        \"serial\": \"41544898.ab7ca8\",\n        \"x\": 162.1428680419922,\n        \"y\": 201.7142848968506,\n        \"wires\": [\n            [\n                \"c033a93.f3fcc58\"\n            ]\n        ]\n    },\n    {\n        \"id\": \"2124191a.a39336\",\n        \"type\": \"serial out\",\n        \"z\": \"4be4e248.b41b1c\",\n        \"name\": \"MySensors Gateway Mega2560 Write\",\n        \"serial\": \"41544898.ab7ca8\",\n        \"x\": 160.42857360839844,\n        \"y\": 246.42858695983887,\n        \"wires\": []\n    },\n    {\n        \"id\": \"41544898.ab7ca8\",\n        \"type\": \"serial-port\",\n        \"z\": \"4be4e248.b41b1c\",\n        \"serialport\": \"/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0\",\n        \"serialbaud\": \"115200\",\n        \"databits\": \"8\",\n        \"parity\": \"none\",\n        \"stopbits\": \"1\",\n        \"newline\": \"\\\\n\",\n        \"bin\": \"false\",\n        \"out\": \"char\",\n        \"addchar\": false\n    }\n]",
        "x": 93.21426391601562,
        "y": 105.99999523162842,
        "wires": []
    },
    {
        "id": "8a5ab1c5.25deb",
        "type": "mqtt in",
        "z": "5a4c4744.8ddec8",
        "name": "Serial In Mega2560",
        "topic": "mysensors/1/out",
        "qos": "2",
        "broker": "26224260.2d8dde",
        "x": 113.21426391601562,
        "y": 182.99999523162842,
        "wires": [
            [
                "53e5263d.054398"
            ]
        ]
    },
    {
        "id": "19b0c05d.0b196",
        "type": "mqtt out",
        "z": "5a4c4744.8ddec8",
        "name": "Swerial Out Mega2560",
        "topic": "mysensors/1/in",
        "qos": "",
        "retain": "",
        "broker": "26224260.2d8dde",
        "x": 140.21426391601562,
        "y": 236.99999523162842,
        "wires": []
    },
    {
        "id": "c08f501b.d27b8",
        "type": "mqtt in",
        "z": "5a4c4744.8ddec8",
        "name": "Serial In RFM69",
        "topic": "mysensors/2/out",
        "qos": "2",
        "broker": "26224260.2d8dde",
        "x": 117.71426391601562,
        "y": 300.9999952316284,
        "wires": [
            [
                "eb9527d1.0a4bf8"
            ]
        ]
    },
    {
        "id": "3e2aa0d0.ee8c8",
        "type": "mqtt out",
        "z": "5a4c4744.8ddec8",
        "name": "Swerial Out RFM69",
        "topic": "mysensors/2/in",
        "qos": "",
        "retain": "",
        "broker": "26224260.2d8dde",
        "x": 133.71426391601562,
        "y": 358.9999952316284,
        "wires": []
    },
    {
        "id": "26224260.2d8dde",
        "type": "mqtt-broker",
        "z": "",
        "broker": "192.168.92.5",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "verifyservercert": false,
        "compatmode": false,
        "keepalive": "15",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "false",
        "birthPayload": "",
        "willTopic": "",
        "willQos": "1",
        "willRetain": "false",
        "willPayload": ""
    }
]
