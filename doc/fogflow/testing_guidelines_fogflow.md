This project is part of FIWARE. For more information check the FIWARE Catalogue entry for Processing.

# FogFlow: non functional test guidelines

FogFlow is an IoT edge computing framework to automatically orchestrate dynamic data processing flows over cloud and edges driven by context, including system context on the available system resources from all layers, data context on the registered metadata of all available data entities, and also usage context on the expected QoS defined by users.

The description of the non functional tests carried out on this implementation is also published via the GitHub repository at Fiware/test.NonFunctional.


## Testing environment

For the testing environment, is needed:

A machine for the deployment of the GEri to be tested, FogFlow. We recommend to use the Docker compilation, where you can find everything necessary to run this environment.

A machine for JMeter, the tool used to inject load and for simulating the client and collecting the results generated by FogFlow.

## Test execution 

### Preliminary setup 

Once the HW necessary for the test described previously at "Testing Environmment" chapter have been setup, the following preliminary steps need to be accomplished before to start the test process: In order to do the installation of FogFlow, a Docker instance is needed.

## Testing step by step 

The test of FogFlow must be done in three steps:

Script configuration:
Depending on the scenario to execute (stress, stress only updates, stability), number of threads, ramp-up and test duration must be configured. Host IP must be configured too. For this purpose, the following User Defined variables in the Jmeter scripts (FogFlow_2.0.0_stability.jmx, FogFlow_2.0.0_stress.jmx, FogFlow_2.0.0_stress_only_update) must be configured:

* HOST -> IP or hostname of the host where FogFlow is deployed.
* PORT -> Port where FogFlow/Broker is listening.
* ABSOLUTE_PATH -> Path for jmx and csv files.
* ATTRIBUTE -> Attribute variable to set when a new context is created or modified.
* tempCounter -> Counter to generate ID's for new contexts.

Furthermore, the number of threads, the ramp up and the test duration must be configured. It is recommended to use the planificator from the threads group. The duration for the stress tests was 25 minutes, and 6 hours in the stability case.

Finally, in order to get the results, the path to the results file must be configured in the Simple Data Writer from the script.

Script execution:

For executing the script, the following command must be typed in the jmeter bin folder:

`./jmeter.sh -n -t /path/to/script/scriptName.jmx`

It´s recommended to execute the command in background and nohup mode:

`nohup ./jmeter.sh -n -t /path/to/script/scriptName.jmx &`

It´s highly recommended to use a hardware resources monitoring tool, in order to measure things like memory and CPU usages, free memory, etc.

## Expected results

As output from the script, a .dat file (in csv format) is generated. This file can be used for plotting different kinds of charts, like threads number, response times, number of errores, responses per second, etc. A plotting tool (like gnuplot) is needed in order to do this.

Additionally, if a monitoring tool has been used, then different data from it is collected too. Depending on the monitoring tool, it can generate charts directly, or it can be used another tool for plotting the output from monitoring tool (for example, sar from systat library can be used for monitoring, and k-sar tool for plotting the sar output)