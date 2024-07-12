# ilsos-voters-sapi
![Powered by](https://img.shields.io/badge/Powered%20by-Mulesoft-535597.svg)
<br>

Voters System API

## Table of contents
1. [Description](#description)
1. [Endpoints](#endpoints)
    1. [PATCH /v1/drivers](#patch-v1drivers)

## Description
Description...

This service implements the next API specification: https://anypoint.mulesoft.com/exchange/0fa744b1-1284-46c5-b23c-0eb98ea787e3/ilsos-drivers-sapi/minor/1.0/

## Endpoints
The service provides the following endpoints:

### POST /v1/registration
Description

The next diagram shows the business sequence of messages or events exchanged between the several backend systems.

```mermaid
sequenceDiagram
    autonumber
    participant ui as UI
    participant api as ilsos-drivers-sapi
    participant db2 as DB2
    participant mainframe as MainFrame

    ui->>api:PATCH/drivers <br>Input:idTransaction,dl,Id,last4ssn,DOB<br>Street,City,State,ZIP and County
    note over db2:DP_ADDRCHG_TRANS
    note over mainframe:CICS:dsf02gOut
    api-->>api:Dataweave - format records for db2<BR> DP_ADDRCHG_TRANS TABLE.
    api-->>db2:Update
    api-->>api:Log response. If db2 access error, then send email to admin
    alt Error Scenario 
        api-->ui: Status 400 , detail error message
    end
    api-->>api:Dataweave - format records for mainframe CICS(dsf02gOut)
    api-->>mainframe:Update driver record
    mainframe-->>api:Retrieve CICS code.
    api-->>api:Log response. If mainframe access error, then send email to admin
    alt Success Scenario 
        api-->ui: Status 200 
    end
    alt Error Scenario 
        api-->ui: Status 400 , error from CICS
    end
  ```
