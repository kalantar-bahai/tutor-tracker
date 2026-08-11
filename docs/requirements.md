
This document contains:

1. [Goals](#goals)
2. [Challenges](#challenges)
3. [Use Cases](#use-cases)
4. [Comments](#comments)
5. [User Management](#user-management)
6. [Permissions Matrix](#permissions-matrix)
7. [Non-Functional Requirements](#non-functional-requirements)
8. [Review of, and Relationship to, Existing Tools](#review-of-and-relationship-to-existing-tools)
9. [Open Questions](#open-questions)(#review-of-and-relationship-to-existing-tools)
10. [Extensions](#extensions)
11. [Glossary](#glossary)
12. [Appendicies](#appendices)

## Goals

A tool, accessible by mobile device, that allows a group of tutors to collectively track the progess of individuals through their study of institute courses. This tool facilitates the recording and analysis of progress over time.

The tool provides the data that allows a coordinator to plan camps that offer study of the most relevant material.

The tool provides input to the Statistical Reporting Program (SRP) which also tracks study circle participation at a lower level of granularity.

## Challenges

The study circle model is one in which a group of participants, the the assistance of a tutor, come together regularly to study books of the training  institute.

In practice, a participant may participate in multiple study cirlces studying the same book but at different points in the book. They may start in one but be unable to advance with other participants because of scheduling conflicts.

## Use Cases

### Use Case A: A tutor plans/starts a new study circle with a group of participants

The {tutor, coordinator} creates a study circle identifying the: book to be studied, the start date, a location, a nickname, any co-tutors and a list of participants.

_Note: If the study circle has already been recorded in SRP, it should not be created here; it should be imported. See Use Case M._

_Note: If the individuals joining the study circle are not already in SRP, records for them will need to be created._

### Use Case B: A tutor studies with a group of participants

After the study is completed, the tutor looks up the study circle (creates it if necessary, see use case A) and:
-  Indicates who participated. If necessary, adds new participants.
-  Identifies any co-tutors. If necessary, add new co-tutors.
-  Identfies which sections were studied. This may include sections already studied in the past.
-  Adds notes (reflections) on the study session.
-  Adds notes (reflections) on particular participants, if desired.

If this is the study circle is new or there are any new tutors or participants then the study circle should be written to SRP on "save".  If the study circle is being created for the first time, a (SRP record) comment should be added as in Use Case M; something like:

```
--- study circle owned by: https://www.tutor-tracker/sc/ID
--- DO NOT EDIT MANUALLY
```

### Use Case C: A new person joins a study circle
  
A {tutor,coordinator} selects the study circle, navigates to the list of participants and adds a new person. 

_Note: This just a subset of Use Case B._

### Use Case D: A person stops attending a study circle
  
A {tutor,coordinator} selects the study circle, navigates to the list of participants and removes the participant from the list.

_Note: This is not strictly required. It reduces the size of the list of participants to review after each study session._

_Note: This is just a subset of Use Case B._

### Use Case E: A study circle stops meeting

If necessary, record progress (use case B).
The tutor / coordinator deletes the study circle. Default is today.

The corresponding record in SRP is updated as follows:

- marked completed
- a (SRP record) comment `#suspended` should be added if no participant completed all of the material in the course, at least some portion in the context of this study circle
- individuals who did no complete the course material are marked as "not complete"

_Note: The study circle is not physically delete; only marked deleted. A date is associated with this state change._

### Use Case F: A participant studies

During or after a study session, the participant selects the book/unit/section studied and records any reflections on the material they studied or plans they make.

_Note: This is not tied to a particular study circle since it is about the material._

### Use Case G: A participant reflects on their study

Participant either selects their study narrative or a particular book.

For book - sees visual representation of all sections completed and any with notes. They can select a particular section and review their notes. They can add more notes.

For narrative - sees section ordered notes. can scroll through and edit any.

### Use Case H: Coordinator reviews progress of a study circle

Coordinator select study circle (from searchable list of study circles).

Coordinator sees visual representation of the study circle, which shows sections studied and number of participants in each section.

Can see alternate represnetation of all participants progress through the book (in all study circles)

Can see any notes they have made in the past. Can add notes.
Can see any notes the tutor has made about the book or by section.

### Use Case I: Coordinator reviews experience of a tutor

Coordinator selects tutor (from list of all tutors (active and inactive)) and book.

Coordinatopr sees visual summary of the tutors experience tutoring the content of the book.

Coorinator can see their notes about a tutor or can drill down and see any notes the tutor has made about the various sections of the book.

Coordinator can add notes about the tutor.

### Use Case J: Tutor reviews the progress of a participant

Tutor selects person (either by selecting book then individual or from list of all individuals in all study circles).

For a given book, they see a summary (grid?) of the participant's progress through the book. This progress is across all study circles the individual has participate in.

Tutor can drill down on a unit/section basis and see:

- when the participant studied the section (may be multiple and with whom (tutor))
- any notes (reflections) from any tutors about the individual(attributed)

_Note: applies to coordinator role as well._

### Use Case K: Coordinator plans a camp
  
A coordination selects camp planning tool. Using it, she identifies a list of registered participants and specifies the time/duration of the camp.

The tool suggests a set of (new) study with these goals:
    
- Each study circle has at least 2 members
- There is a list of contiguous sections in a book that will enable all of the members in the study circle to increase their coverage of the book.

The coordinator can work interactively assigning tutors or identifying a book as undesirable because no tutor can be found.

### Use Case M: Coordinator imports study circle(s) from SRP

Coorindator sees list of study circles in SRP. Identifies one or more and selects "import". Details about each selected study circle are imported to the tool and a link identifying the tool record is written back to SRP. Something like: identifying the tool and an identifier.

```
--- study circle owned by: https://www.tutor-tracker/sc/ID
--- DO NOT EDIT MANUALLY
```

## Comments

- A tutor of a study circle can be a participant in another
- A coordinator responsible for a study circle can be a tutor or a participant
- A tutor must be able to see details about the participants in study circles for which they are tutors; they must not be able to see details about others
- A coordinator can see full details (including tutor notes) about all study circles and all particpants
- Everyone should be able to see the list of study circles but not the list of particpants
- A participant can see a list of study circles they are participating in or have participated in
- A participant can see their progress in any given book
- We can assume that the set of study circle books is known a priori. However, we should expect, over time, that this set might change.
- The tool has metadata about study circle books but does not maintain their content.
- Given the limitations of [SRP integration](#srp-integration), individual records will need to be cached. Ownership remains with SRP (as with Activities). Not all information stored here (ex., notes) needs to be pushed to SRP.

### SRP Integration

[SRP](#statistical-report-program-srp) does not provide direct database access or an API. Therefore, the only integration options are:

- _Fully manual_ -- tool generates a report changes and the coordinator manually updates  SRP.
- _Assisted automation_ -- coordinator logs into SRP (handles 2FA), then runs a (Playwright) script to implement the list of changes.
- _Full automation_ -- tool owns the session - stores credentials, handles
2FA, and drives the whole flow unattended when the change is made (or later via an administrative action).

Only the third option can satisfy the spirit of use cases A, B, E and M. However, the first two are also options if a log of changes is produced.

If either of the second or third option is selected, a reusable SRP "API" (over Playwright) for individuals and study cirlces (perhaps all activities) should be defined and implemented -- for separation of concerns and for potential re-use in future projects.

## Permissions Matrix

Roles below are: **coordinator**, **facilitator**, **participant**._

| Resource / Action | Coordinator | Facilitator | Participant |
|---|---|---|---|
| View list of study circles | Full | Full | Full (list only — see Comments) |
| View study circle details (incl. tutor notes) | Full (see Comments) | Own study circles only (see Comments) | list details  only |
| View participant list within a study circle | Full | Own study circles only (see Comments) | None (see Comments) |
| Create / edit study circle (Use Case A) | Full | Own study circles only | None |
| Delete (mark deleted) a study circle (Use Case E) | Full | Own study circles only | None |
| Record a study session — attendance, sections studied, session notes (Use Case B) | Full | Own study circles only | None |
| Add / remove participant in a study circle (Use Cases C, D) | Full | Own study circles only | None |
| List individuals | Full | List only | None |
| Inspect individual | Full | Summary details only | Own |
| Record own study progress / reflections (Use Case F) | N/A | N/A | Own only |
| View own progress and notes across all study circles (Use Case G) | N/A | N/A | Own only |
| Add / view notes about a participant | Full (view all, add) | Own study circles only (add); attributed notes from other tutors are visible (see Use Case J) | None |
| Add / view notes about a book or section (tutor's notes) | Full (view) | Own (add) | None |
| Add / view notes about a tutor (coordinator's notes) | Full | None | None |
| Review tutor experience summary (per Use Case) | Full | Own experience only | None |
| Plan a camp (Use Case K) | Full | None | None |
| Imports study circle(s) from SRP (Use Case M) | Full | None | None |

## User Management

Only approved users can use the tool. 

- a user login in via a third party 2FA provider such as google
- a user is a valid user if their email address appears in the record of an individual
- a coordinator when creating a study circle is identifying a tutor; ie, giving them the tutor role

## Non-Functional Requirements

### Scale

This is a single cluster pilot in the context of a region. If successful, it will be rolled out to approximately 10 clusters in the same region. The largest cluster currently has about 75 active tutors facilitating 100 study circles with 400 participants. There about bout 250 potential tutors and 1500 total people.  We should assume growth in that cluster by 10x over the next 5 years.

### Connectivity

Primarily a tool for mobile device use. Assumes connectivity.
 
### Data Sensitivity

Information about individuals should be conisdered private should be visible strictly following the permissions matrix.

### Availability

Service should be available 24/7.

### Auditability

An audit record should be created for all logins/writes.

### Data retention

### Backup / disaster recovery

SRP serves as a backup of some, but not all information.

### Localization

Primary user languages are English and Spanish.

## Open Questions

- What does it mean to complete a study circle?  Is it that a tutor must mark every section of a book studied or is it a percentage?  There are also "practices" (not tracked formally in tool); does it include a requirement to attempt these?

## Review of, and Relationship to, Existing Tools

### Statistical Report Program (SRP)

SRP is a tool that tracks individuals progress through books of the institue. It contains _individual_ records and _activity_ records. An activity record links describes a study circle linking the participants and tutor(s) to a book.  Technical details are [here](SRP.md).

This level of granularity present in SRP is not sufficient for tracking progress because participants do not consistently stay in one study circle through the study of a book. Instead, their study is fragmented and occurs in many study circles.

SRP will remain the primary tool for communicating the growth of a cluster to others. In particular, this means that a subset of the information we keep must be written to SRP.

### Youth Progression (Google sheet)

The cluster coordinator maintains a spreadsheet identifying participation in monthly "camps" (study cirlces).  Like SRP, participation is recorded at the book level only.

It is not expected that this tool interacts at all with the Youth Progression spreadsheet.

## Extensions

### Other Clusters

This is a single cluster pilot. If successful, it will be rolled out to additional clusters in the same region. The data model should minimally support this plan.

### Other Educational Programs

A similar educational program exists for _children's classes_ and _junor youth groups_. Terminology changes but the basic features of a system to track progress would remain the same. This could be part of the same product or be companion products.

Further, there are additonal study materials such as Preparation for Social Action (PSA) and Institute for Global Prosperity (ISGP) that could be considered activity types.

The first version of this tool should address study circles only. However, it should be designed with the intent to extend to related educational activities. Where possible, we should generalize the model but not the presentation.

For reference, the terminology maps as follows:

- study circle -> children's class, junior youth group
- tutor -> teacher, animator
- book -> grade, text

Unlike study circle books, children's class grades and junior youth texts do not have units.

## Glossary

- *tutor*, *facilitator* - an individual who accompanies _participants_ through the study of a _book_ in the form of a _study circle_
- *coordinator* - an individual responsible for the coordination of a number of study circles in a cluster
- *study circle* - the approach by which a group of _participants_ study a _book_ together, assisted by a _tutor_
- *book* - a volume of material to be studied in a study circle. Books have a title. Some books are considered "branch courses" of main texts.
- *unit* - part of a book; books have 2 units. Titled. Each unit has a purpose and a number of sections.
- *section* - a part of a study circle unit. Numbered. Untitled.
- *activity* - general term for educational activity including study circles

## Appendices

### Current Study Circle Books

Study circle material currently comes from the Ruhi Organization. The current set is:

- Book 1: Reflections on the Life of the Spirit
- Book 2: Arising to Serve
- Book 3: Teaching Children's Classes, Grade 1
- Book 4: The Twin Manifestations
- Book 5: Releasing the Powers of Junior Youth
- Book 6: Teaching the Cause
- Book 7: Walking Together on a Path of Service
- Book 8: The Covenant of Bahá'u'lláh
- Book 9: Gaining an Historical Perspective
- Book 10: Building Virbrant Communities
- Book 11: Material Means
- Book 12: Family and the Community
- Book 13: Engaging in Social Action
- Book 14: Participating in Public Discourse

Branch courses

- Book 3 BR1 -Teaching Children's Classes, Grade 2
- Book 3 BR2 - Teaching Children's Classes, Grade 3
- Book 3 BR2 - Teaching Children's Classes, Grades 4 and 5
- Book 5 BR1 - Initial Impulse
- Book 5 BR2 - Widening Circle
- Book 7 BR1 - Unknown


