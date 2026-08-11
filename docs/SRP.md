# Statistical Report Program (SRP)

## Locations

Models information about various geographic units:

- Regions
- Subregions
- Clusters
- Groups of Clusters
- Localities
- Focus Neighborhoods
- Electoral Units

In general, these are (in order) contained within the parent. However, this is not strictly true. An electoral unit is collection of localities. A focus neighborhood can contains parts of multiple localities.

One can think of individuals or activities as belonging to a location.  However, some activities belong to a nucleus that spans geographic notions.  An individual can participate in multiple places or groups.

## Individuals

Models information about individuals including basic information, contact information, household membership.

## Activities

Models study circles, children's classes and junior youth groups. Associates an activity with period of time, facilitators and participants. Children's classes and junior youth groups track progress though 1 or more texts/grades.

There are activities that are counted but not tracked such as devotional gatherings or home visits. And there are activities not counted at all such as firesides.

## Cycles

Model progress through a cycle. Is a point it time capture of data for a cluster (but not at any other georgraphic level).

## Reports

Queries or views of the data to enable 
Many ("general" and "institute") are predefined and rely of complex join logic between tables. Others ("custom") are over individuals and allow simple and/or logic of fields accessible from an individual.

## Tools

Configuration, profile, user management, audit log, change log.

## Implementation

Is an SQL database. A schema for part of it is [here](/Users/kalantar/projects/github.com/kalantar/neighborhood/docs/srp-database-schema.sql). It is the core schema. It has been extended here and there but these extensions are not included in the schema documents.

## Interaction Model

SRP is an online tool (webapp). 
It does not provide a REST API.
It requires 2 factor authentication.
Can be accessed via playwright.