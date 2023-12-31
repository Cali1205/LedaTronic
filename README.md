# Node-RED Flow - LedaTronic

This is a Node-RED flow named "LedaTronic." The flow is designed to receive data from a TCP server, process it, and then send it over MQTT.

## Components

### TCP In Node

- **Name:** Not specified
- **Server:** Client
- **Host:** 192.168.178.63
- **Port:** 10001
- **Datamode:** Stream
- **Datatype:** Buffer

This node receives data from a TCP server at the specified IP address and port number in stream mode. The received data is expected as a buffer.

### Function Nodes

#### Function Node 1

- This function checks if the first byte value of the received data is equal to 14. If yes, it passes the message as is. Otherwise, it does not forward the message.

#### Function Node 2

- This function checks if the first byte value of the received data is not equal to 14. If yes, it forwards the message to the debug output node without any changes.

### Buffer Parser Node

- **Name:** Not specified
- **Data:** payload
- **DataType:** msg
- **Specification:** spec (UI)
- **Items:** A list of data fields to extract from the received data, including fields like "temp_chamber," "shutter_current," "shutter_target," etc.

This node parses the received data using the specified specification and extracts the specified data fields.

### MQTT Out Node

- **Name:** Not specified
- **Topic:** LedaData
- **Broker:** <MqttBroker>

This node sends the processed data over MQTT with the specified topic name to the broker.

### MQTT Broker Node

- **Name:** <MqttBrokerName>
- **Broker:** <MqttBroker>
- **Port:** 1883
- **Client ID:** Leda

This node establishes a connection to the MQTT .

## Flow chart
```mermaid
graph TD
  A["TCP In: 192.168.178.63:10001"] -->|Data Stream| B["Function: Filter by Payload[0] == 14"]
  B -->|If true| C["Buffer Parser"]
  C -->|Parsed Data| D["MQTT Out: LedaData"]

  B -->|If false| E["Function: Filter by Payload[0] != 14"]
  E -->|Discarded Data| F["Debug"]

```
### Buffer Parser
| Item              | Type      | Name             | Offset | Length | Offset Bit | Scale | Mask |
|-------------------|-----------|------------------|--------|--------|------------|-------|------|
| temp_chamber      | uint16be  |                | 2      | 1      | 0          | 1     |      |
| shutter_current   | uint8     | shutter_current  | 4      | 1      | 0          | 1     |      |
| shutter_target    | uint8     | shutter_target   | 5      | 1      | 0          | 1     |      |
| state             | uint8     | state            | 6      | 1      | 0          | 1     |      |
| error             | uint8     | error            | 7      | 1      | 0          | 1     |      |
| output            | uint8     | output           | 8      | 1      | 0          | 1     |      |
| outputState       | uint8     | outputState      | 9      | 1      | 0          | 1     |      |
| temp_chamber_max  | int16be   |                | 10     | 1      | 0          | 1     |      |


## Usage

This flow receives data from a TCP server, filters it using functions, and extracts specific data fields using the Buffer Parser node. Then, the processed data is sent over MQTT to the broker.

**Note:** Please ensure that the necessary broker and server details are correctly configured to run the flow properly.

This is a basic documentation for your Node-RED flow in Markdown format. You can use it in your GitHub repository README file or customize it further to add more information or explanations.