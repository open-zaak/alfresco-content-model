# Alfresco content model

This repository contains the custom content model in Alfresco, intended
to be used with the Open Zaak CMIS-adapter.

Note that this repository is mainly used for (automated) testing purposes.

Open Zaak does not claim or aspire to provide a production-ready Alfresco
setup.

# Alfresco module informatie

Gebaseerd op het Alfresco SDK4 archetype

Deze module bevat de onderstaande onderdelen.
- Alfresco share configuratie
- Alfresco content model

Daarnaast is er ook een docker test deployment aanwezig gebaseerd op Alfresco 6.2 community. Het is ook mogelijk om dit te wijzigen naar Alfresco 6.2 enterprise, maar hier is wel een Alfresco license voor nodig bij langdurig gebruik.

Het content model en de share configuratie worden gepackaged als .jar bestand. Instructies hiervoor en voor de installatie als module staan hieronder beschreven.

## Vereiste build tools

- Java 1.8 of 11
- Maven 3.3.0 of hoger

## Lokaal draaien

Extra vereiste tools:
- Docker

Stappen:
1. Draai het commando `./run.sh build_start` of `./run.bat build_start`
2. Ga naar http://localhost:8180/share en wacht op een login scherm
3. Login met gebruiker: `admin` en wachtwoord: `admin`

Als het mogelijk is om op Share in te loggen, kan development beginnen.

Merk op dat deze test stack bestaat uit 4 containers. Alfresco, met de CMIS connectie, draait op http://localhost:8080/alfresco. Poortnummers zijn te veranderen in pom.xml.

## Content model gebruiken voor productie

Sinds Alfresco 6 is het de bedoeling dat modules geinstalleerd worden door docker images uit te breiden. Dit is door [Alfresco](https://hub.alfresco.com/t5/alfresco-content-services-blog/deploying-and-running-alfresco-content-services-6-0/ba-p/293225) beschreven. Om deze docker images uit te breiden zijn de jar modules nodig. Bouw deze door het commando `./run.sh build` of `run.bat build` uit te voeren. De Alfresco platform jar staat in `openzaak-alfresco-platform/target` en de share jar staat in `openzaak-alfresco-share/target`

# Alfresco AIO Project - SDK 4.0

This is an All-In-One (AIO) project for Alfresco SDK 4.0.

Run with `./run.sh build_start` or `./run.bat build_start` and verify that it

 * Runs Alfresco Content Service (ACS)
 * Runs Alfresco Share
 * Runs Alfresco Search Service (ASS)
 * Runs PostgreSQL database
 * Deploys the JAR assembled modules
 
All the services of the project are now run as docker containers. The run script offers the next tasks:

 * `build_start`. Build the whole project, recreate the ACS and Share docker images, start the dockerised environment composed by ACS, Share, ASS and 
 PostgreSQL and tail the logs of all the containers.
 * `build_start_it_supported`. Build the whole project including dependencies required for IT execution, recreate the ACS and Share docker images, start the 
 dockerised environment composed by ACS, Share, ASS and PostgreSQL and tail the logs of all the containers.
 * `start`. Start the dockerised environment without building the project and tail the logs of all the containers.
 * `stop`. Stop the dockerised environment.
 * `purge`. Stop the dockerised container and delete all the persistent data (docker volumes).
 * `tail`. Tail the logs of all the containers.
 * `reload_share`. Build the Share module, recreate the Share docker image and restart the Share container.
 * `reload_acs`. Build the ACS module, recreate the ACS docker image and restart the ACS container.
 * `build_test`. Build the whole project, recreate the ACS and Share docker images, start the dockerised environment, execute the integration tests from the
 `integration-tests` module and stop the environment.
 * `test`. Execute the integration tests (the environment must be already started).

# Few things to notice

 * No parent pom
 * No WAR projects, the jars are included in the custom docker images
 * No runner project - the Alfresco environment is now managed through [Docker](https://www.docker.com/)
 * Standard JAR packaging and layout
 * Works seamlessly with Eclipse and IntelliJ IDEA
 * JRebel for hot reloading, JRebel maven plugin for generating rebel.xml [JRebel integration documentation]
 * AMP as an assembly
 * Persistent test data through restart thanks to the use of Docker volumes for ACS, ASS and database data
 * Integration tests module to execute tests against the final environment (dockerised)
 * Resources loaded from META-INF
 * Web Fragment (this includes a sample servlet configured via web fragment)

# TODO

  * Abstract assembly into a dependency so we don't have to ship the assembly in the archetype
  * Functional/remote unit tests

