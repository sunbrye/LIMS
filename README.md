# Simplified Laboratory Information Management System

## Contents
1. [Introduction](#Introduction)
2. [Workflow](#Workflow)
   * [Order Creation](#Order-Creation)
   * [Specimen Arrival](#Specimen-Arrival)
   * [Extraction](#Extraction)


### Introduction 
A Laboratory Information Management System (LIMS) is software that allows you to effectively manage samples and associated data. By using a LIMS, your lab can automate workflows, integrate instruments, and manage samples and associated information. Additionally, you can produce reliable results more quickly and can track data from sequencing runs over time and across experiments to improve efficiency.

Modern genomics generates an unprecedented amount of data. Faced with increasing data volumes and sample throughput along with frequent changes in technology, labs must modernize their approach to managing, tracking, and centralizing genomics data.

### Workflow
##### Order Creation 
Workflows begin once a physician or patient place an order. A **requisition** (RQ)  is created within an internal software system that contains information about the patient and order/tests requested. This generates a work order that is used to track the overall progress of the RQ. 
```yaml
paths:
  /requisition/{id}:
    get:
      summary: Returns a requisition by ID.
      parameters:
        - in: path
          name: id
          required: true
          type: uuid
      responses:
        200:
          description: OK
          schema:
            $ref: '#/definitions/requisition'
  /requisition:
    post:
      summary: Creates a new requisition.
      parameters:
        - in: body
          name: requisition
          schema:
            $ref: '#/definitions/requisition'
      responses:
        200:
          description: OK
```


##### Specimen Arrival 
**Specimens** are shipped to the lab and must first go through accessioning which handles the intake, labeling and preliminary review of the specimen and requisition. When a specimen is accessioned, the physical specimen is matched to an associated requisition and patient information such as collection date, tube size, specimen type (TODO turn this into a list) is inputted. 
```yaml
paths:
  /specimen/{id}:
    get:
      summary: Returns a specimen by ID.
      parameters:
        - in: path
          name: id
          required: true
          type: uuid
      responses:
        200:
          description: OK
          schema:
            $ref: '#/definitions/specimen'
  /specimen:
    post:
      summary: Creates a new specimen and matches it to a requisition.
      parameters:
        - in: body
          name: specimen
          schema:
            $ref: '#/definitions/specimen'
      responses:
        200:
          description: OK
```

##### Extraction
**Extraction** is a method to purify DNA from a sample by using physical and/or chemical methods. The lab performs this work on samples that have been accessioned. Magnetic seperation is used by binding DNA to magnetic beads. This seperates the DNA from the sample. The **quality** and yield of DNA are assessed by spectrophotometry or by gel electrophoresis.

```yaml
paths:
  /specimen/extraction:
    post:
      summary: Creates a new extraction associated with a specimen.
      parameters:
        - in: body
          name: specimen
          required: true
      responses:
        200:
          description: OK
          schema:
            $ref: '#/definitions/specimen/extraction'
  /specimen/quality_control:
    post:
      summary: Creates a new quality control report.
      parameters:
        - in: body
          name: specimen
          schema:
            $ref: '#/definitions/specimen/quality_control'
      responses:
        200:
          description: OK
```

##### Batching

##### Library Prep

##### Sequencing 

##### Reads to Variants

##### Confirmation

##### Report 
