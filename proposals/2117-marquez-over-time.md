# Proposal: Marquez API to retrieve historical data

## Problem

Marquez data model in PostgresSQL allows extracting a snapshot of any lineage content from the past. This may be extremely
useful to track how job, dataset etc. evolved over time. The feature can be implemented within each Marquez endpoint: like
retrieving datasets or jobs. The purpose of this doc is to agree on a consistent API implementation approach. 

## Solution

Point in time can be described by a datetime, a dataset version, a job version or even by runId. We decide that versions are the
only reliable parameter for a point in time lineage API. Datetime can be ambiguous, for instance by having START and COMPLETE
datetime for each job.

We assume the ability to scan over time can be implemented within UI. 

We introduce `snapshotAt` URL parametere and provide below some example values of it:

 * `/api/v1/namespaces/some-namespace/datasets/some-dataset?snapshotAt=dataset_version:5ca3b37e-4e18-11ed-bdc3-0242ac120002`
 * `/api/v1/namespaces/some-namespace/jobs/some-job?snapshotAt=job_version:5ca3b37e-4e18-11ed-bdc3-0242ac120002`
 * `/api/v1/namespaces/some-namespace/jobs/some-job?snapshotAt=run_id:5ca3b37e-4e18-11ed-bdc3-0242ac120002`
 
Please mind that requested version id has to correspond to requested entity. In the first example dataset_version
has to be a version of `some-dataset`. 

### Questions

* *Shall a response contain a point in time marker?* This would have been helpful but can be difficult to incorporate
into existing endpoints, especially if the root node of the JSON response is an array. 
