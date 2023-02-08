# Usage

- [Usage](#usage)
  - [Read metadata](#read-metadata)
  - [Write data](#write-data)
  - [Read data](#read-data)
  - [Use Data Service](#use-data-service)
  
Via the IE Flow Creator, we can write and read the OPC UA data.

The used flow can be downloaded [here](/src/flow.json) and imported into the IE Flow Creator, that is running on the same IED as the OPC UA Connector.

## Read metadata

To print out the OPC UA Connector metadata, follow these steps:

- create a mqtt in node
- set the server to 'ie-databus' with port 1883 and corresponding user name/password ('edge'/'edge')
- set the topic to `ie/m/j/simatic/v1/opcuac1/dp`
- create a debug node and connect to the mqtt in node
- deploy the flow and watch the debug window

![metadata_flow](/docs/graphics/Metadata_Flow.png)

![metadata](/docs/graphics/Metadata.png)

Now you can see the configured datapoints according to OPC UA Configurator settings.


## Write data

To write some data on the OPC UA tags, you must fetch the tag ID from metadata payload based on the tag name. Please follow these steps:

- for the ***intValueEdge2Plc*** tag, create an inject node to write the value 4444 with this JSON payload: `{"seq":1,"vals":[{"id":"103","qc":3,"val":4444}]}`
- for the ***intValueEdge2Plc*** tag, create another inject node to write the value 9999 with this JSON payload: `{"seq":1,"vals":[{"id":"103","qc":3,"val":8888}]}`
- check if the ***intValueEdge2Plc*** data point has the ID 103, change it to your value if not
- create a mqtt out node
- set the server to 'ie-databus' with port 1883 and corresponding user name/password ('edge'/'edge')
- set the topic to `ie/d/j/simatic/v1/opcuac1/dp/w/Profibus2Edge`, change Profibus2Edge with your OPC UA connection name if you used another one
- connect the inject nodes to the mqtt out node
- deploy the flow
- click the single inject buttons, to write the values

![write_data_flow](/docs/graphics/Write_Data_Flow.png)

## Read data

To print out the transfered OPC UA Connector data, you must fetch the tag ID from metadata payload based on the tag name. Please follow these steps:

- create a mqtt in node
- set the server to 'ie-databus' with port 1883 and corresponding user name/password ('edge'/'edge')
- set the topic to `ie/d/j/simatic/v1/opcuac1/dp/r/#`
- create a debug node and connect to to the mqtt in node
- deploy the flow
- as soon as the value for a configured tag changes, you can see it in the debug window

![read_data_flow](/docs/graphics/Read_Data_Flow.png)

The incoming data looks like the following:

![read_1](/docs/graphics/Read_1.png)

## Use Data Service

The app Data Service collects the data out of different connectors and stores it for a defined time period. This is a prerequisite for other apps like Performance Insight.

To activate the data transfer from the OPC UA Connector, proceed as following:

- open the IED web interface
- open the app Data Service
- go to tab 'Connectors' and select 'OPC UA Connector'
- select the edit button and enter the user name and the password for the Databus user ('edge'/'edge')
- activate the adapter and save

![DataServiceAdapter](/docs/graphics/DataService_Adapter.png)

- go to tab 'Assets & Connectivity' and add the variables, that were configured within the OPC UA Connector

![DataServiceAdapter](/docs/graphics/DataService_Add.png)

- data is now collected by the Data Service and can be used by further apps