# ilsos-voters-sapi
![Powered by](https://img.shields.io/badge/Powered%20by-Mulesoft-535597.svg)
<br>

Voters System API

## Table of contents
1. [Description](#description)
1. [Endpoints](#endpoints)
    1. [POST /v1/voters/registration](#post-v1voters)

## Description
Description...

This service implements the next API specification: https://anypoint.mulesoft.com/exchange/0fa744b1-1284-46c5-b23c-0eb98ea787e3/ilsos-voters-sapi/minor/1.0/

## Endpoints
The service provides the following endpoints:

### POST /v1/voters/registration
Register the address for a voter.

The next diagram shows the business sequence of messages or events exchanged between the several backend systems.

```mermaid
sequenceDiagram
    autonumber
    participant ui as UI
    participant api as ilsos-voters-sapi
    participant db2 as DB2
    
    ui->>api:POST/voters/registration <br>Input: idTransaction,dl,Id,last4ssn,DOB<br>Street,City,State,ZIP and County
    note over db2:DS_BOE_XREF_EXISTS<br>DS_WEB_AVR
    api-->>api:Dataweave - format records for db2<BR> DS_BOE_XREF_EXISTS STORED PROCEDURE<br>DS_WEB_AVR TABLE
    api-->>db2: Execute and insert 
    db2-->>api: voter info
    api-->>api:Log response. If db2 access error, then send email to admin
    alt Success Scenario 
        api-->ui: Status 201 , idtransaction
    end
    alt Error Scenario 
        api-->ui: Status 400 , detail error message
    end
  ```
