<A NAME="top">
    
# Blue-Green Service Upgrade Process

## Sec 1 : Purpose

Enterprise Connect employs a "[Blue-Green](https://docs.cloudfoundry.org/devguide/deploy-apps/blue-green.html)" approach to upgrading existing services for its users. This document endeavors to provide insight to that procedure for our users, as well as to serve as a how-to guide internally, for posterity. 

## Sec 2 : Scope

This procedure applies to updating any existing Enterprise Connect Service within the following environments:

- Predix "West" / Basic
- Predix "East" / Select
- CF3

## Sec 3 : Instructions

### 1. Determine the build parameters:
1. Identify the Zone ID(s) for the Service(s) to be updated
2. Identify the desired Service version to be used (e.g., okinawa.130, sendai.120, etc.)

### 2. Locate appropriate Jenkins jobs (soon-to-be deprecated and in flux):
- 'Prod' for Services with domain run.aws-usw02-pr.ice.predix.io
- 'Select' for Services with domain run.asv-pr.ice.predix.io
- 'CF3' for Services with domain run.aws-usw02.dev.ice.predix.io

### 3. Supply the build parameters:
- Zone ID(s) from Step 1 provided as comma-separated-values
- Service version 

### 4. Run the build
Assuming the build is successful, at this point the user/customer should be manually notified that an updated service has been provisioned for their use and testing. The EC team member should outline the implications. 