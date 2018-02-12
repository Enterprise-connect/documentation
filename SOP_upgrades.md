<A NAME="top">
    
# Blue-Green Service Upgrade Process

This document is currently a work in progress due to an imminent deprecation of the Jenkins instance we use to perform the steps explained herein. As the migration is finalized, the instructions will be updated to reflect more precisely the steps involved.

## Sec 1 : Purpose

Enterprise Connect employs a "[Blue-Green](https://docs.cloudfoundry.org/devguide/deploy-apps/blue-green.html)" approach to upgrading existing services for its users. This is the documentation for the process.

## Sec 2 : Scope

This procedure applies to updating any existing Enterprise Connect Service within the following environments:

- Predix "West" / Basic
- Predix "East" / Select
- CF3

## Sec 3 : Instructions

### 3.1 Determine the build parameters:
1. Identify the Zone ID(s) for the Service(s) to be updated
2. Identify the desired Service version to be used (e.g., okinawa.130, sendai.120, etc.)

### 3.2 Locate appropriate Jenkins jobs (soon-to-be deprecated and in flux):
- 'Prod' for Services with domain run.aws-usw02-pr.ice.predix.io
- 'Select' for Services with domain run.asv-pr.ice.predix.io
- 'CF3' for Services with domain run.aws-usw02.dev.ice.predix.io

### 3.3 Supply the build parameters:
- Zone ID(s) from Step 1 provided as comma-separated-values
- Service version 

### 3.4 Run the job
At this point, several considerations are needed based on user specifications/requests:

#### 3.4.1 The user has requested that only Step 1 be performed:
If the user has specified that they only want step 1 to be performed
