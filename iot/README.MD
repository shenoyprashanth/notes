### Devices
- Are internet enabled.
- Low power devices that cannot be constantly connected.
- Are not internet enabled.

### Software
- Operating system such as Windows IOT core or Amazon Free RTOS.
- SDK for connecting to the cloud gateway
- SDK for allowing applications to connect to the devices.

### Field Gateway
- Low power devices can connect to a field gateway which can then push the telemetry data to cloud.
- Also isolates the device since it is not directly exposed over the internet.

### Edge
- Process the data on the edge before sending it to the cloud.
- Batch the data , compress the data - Keeps the cost low.
- AWS IOT Green Grass provides capabilities such as machine learning, offline data storage etc.

### Protocol Gateway
- Support wide range of devices by translating the protocols. (Custom, AMQP, MQTT etc.)

### Authentication and Authorization
- Mutual SSL, Encryption

### Device Registry
- Registry of all the "things"

### Stuff you do on the cloud
- **Rules Engine** : Transform messages based on rules and route it to different destinations.
- **Streaming Analytics** : Run SQL like queries on streams.
- **Predictive analytics**
- **Anomaly detection**

### Visualization
- BI dashboard

### Troubleshooting
- Connecting to device. 
- View device logs.



