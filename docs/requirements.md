
This document contains:

1. [Goals](#goals)
2. [Challenges](#challenges)
3. [Use Cases](#use-cases)
4. [Comments](#comments)
5. [Open Questions](#open-questions)
6. [Limitations of Existing Tools](#limitations-of-existing-tools)
7. [Extensions](#extensions)
8. [Glossary](#glossary)
9. [Appendicies](#appendices)

## Goals

A tool, accessible by mobile device, that allows a group of tutors to collectively track the progess of individuals through their study of institute courses. This tool facilitates the recording and analysis of progress over time.

The tool provides the data that allows a coordinator to plan camps that offer study of the most relevant material.

The tool provides input to the Statistical Reporting Program (SRP) which also tracks study circle participation at a lower level of granularity.

## Challenges

The study circle model is one in which a group of participants, the the assistance of a tutor, come together regularly to study books of the training  institute.

In practice, a participant may participate in multiple study cirlces studying the same book but at different points in the book. They may start in one but be unable to advance with other participants because of scheduling conflicts.

## Use Cases

#### A: A tutor plans/starts a new study circle with a group of participants

The {tutor, coordinator} creates a study circle identifying the: book to be studied, the start date, a location, a nickname, any co-tutors and a list of participants.

#### B: A tutor studies with a group of participants

After the study is completed, the tutor looks up the study circle (creates it if necessary, see use case A) and:
-  Indicates who participated. If necessary, adds new participants.
-  Identifies any co-tutors. If necessary, add new co-tutors.
-  Identfies which sections were studied. This may include sections already studied in the past.
-  Adds comments (reflections) on the study session.
-  Adds comments on particular participants, if desired.

#### C: A new person joins a study circle
  
A {tutor,coordinator} selects the study circle, navigates to the list of participants and adds a new person.

#### D: A person stops attending a study circle
  
A {tutor,coordinator} selects the study circle, navigates to the list of participants and removes the participant from the list.

_Note: This is not strictly required. It reduces the size of the list of participants to review after each study session._

#### E: A study circle stops meeting

If necessary, record progress (use case B).
The tutor / coordinator deletes the study circle. Default is today.

_Note: Unlike SRP, there is no need to mark participants completion; this has already been done via use case B._

_Note: The study circle is not physically delete; only marked deleted. A date is associated with this state change._

#### F: A participant studies

During or after a study session, the participant selects the book/unit/section studied and records any reflections on the material they studied or plans they make.

_Note: This is not tied to a particular study circle since it is about the material._

#### G: A participant reflects on their study

Participant either selects their study narrative or a particular book.

For book - sees visual representation of all sections completed and any with notes. They can select a particular section and review their notes. They can add more notes.

For narrative - sees section ordered notes. can scroll through and edit any.

#### Coordinator reviews progress of a study circle

Coordinator select study circle (from searchable list of study circles).

Coordinator sees visual representation of the study circle, which shows sections studied and number of participants in each section.

Can see alternate represnetation of all participants progress through the book (in all study circles)

Can see any notes they have made in the past. Can add notes.
Can see any notes the tutor has made about the book or by section.

#### Coordinator reviews experience of a tutor

Coordinator selects tutor (from list of all tutors (active and inactive)) and book.

Coordinatopr sees visual summary of the tutors experience tutoring the content of the book.

Coorinator can see their notes about a tutor or can drill down and see any notes the tutor has made about the various sections of the book.

Coordinator can add notes about the tutor.

#### Tutor reviews the progress of a participant

Tutor selects person (either by selecting book then individual or from list of all individuals in all study circles).

For a given book, they see a summary (grid?) of the participant's progress through the book. This progress is across all study circles the individual has participate in.

Tutor can drill down on a unit/section basis and see:

- when the participant studied the section (may be multiple and with whom (tutor))
- any comments from any tutors aobut the individual(attributed)

_Note: applies to coordinator role as well._

#### Coordinator plans a camp
  
    Given:
    -  a list of participants
    -  a number of study sessions
    
Divide the set of participants into groups such that:

- Each group has at least 2 members
- There is a list of contiguous sections in a book that will enable all of the members in the group to increase their coverage of the Identify a set of books sections that should be studied in the camp such that

    - visualize the participants experience with the book
    - identify subset of particpants 

    - propose list of sets of participants together with a list of books/sections (contiguous) 

#### Coordinator updates SRP

Coordinator triggers can administrative action -- update SRP.

For each study circle (including recently delete), find the study circle in SRP.

If it does not exist, create a new study circle in SRP.

If it exists, update lists of tutors, participants if necessary.

If has been deleted, mark study circle completed in SRP. Mark individuals `complete` if they have completed the course.

_Note: this implies there is a reliable way to map study circles in the tool with those in SRP._

## Comments

- A tutor of a study circle can be a participant in another
- A coordinator responsible for a study circle can be a tutor or a participant
- A tutor must be able to see details about the participants in study circles for which they are tutors; they must not be able to see details about others
- A coordinator can see full details (including tutor notes) about all study circles and all particpants
- Everyone should be able to see the list of study circles but not the list of particpants
- A participant can see a list of study circles they are participating in or have participated in
- A participant can see their progress in any given book
- We can assume that the set of study circle books is known a priori. However, we should expect, over time, that this set might change.
- Updates to SRP are not immediate. They require an administrative action by the coordinator.

## Open Questions

- What does it mean to complete a study circle?
- Should SRP updates be instantaneous?
- Should individuals be cached from SRP or read directly?
- How general is the data model in the tool? Does it support just this tool or does it support other tools as well?

## Limitations of Existing Tools

### Statistical Report Program (SRP)

SRP is a tool that tracks individuals progress through books of the institue. It contains _individual_ records and _activity_ records. An activity record links describes a study circle linking the participants and tutor(s) to a book.

This level of granularity is not sufficient for tracking progress because participants do not consistently stay in one study circle through the study of a book. Instead, their study is fragmented and occurs in many study circles.

### Youth Progression (Google sheet)

The cluster coordinator maintains a spreadsheet identifying participation in monthly "camps" (study cirlces).  Like SRP, participation is recorded at the book level only.

## Extensions

A similar educational program exists for _children's classes_ and _junor youth groups_. Terminology changes but the basic features of a system to track progress would remain the same. This could be part of the same product or be companion products.

### Terminology Mapping

- study circle -> children's class, junior youth group
- tutor -> teacher, animator
- book -> grade, text

Unlike study circle books, children's class grades and junior youth texts do not have units. They do have 

## Glossary

- *tutor*, *facilitator* - an individual who accompanies _participants_ through the study of a _book_ in the form of a _study circle_
- *accompanier* - an individual who accompainies a group of tutors as they gain experience facilitating study circles
- *coordinator* - an individual responsible for the coordination of a number of study circles in a cluster
- *study circle* - the approach by which a group of _participants_ study a _book_ together, assisted by a _tutor_
- *book* - a volume of material to be studied in a study circle. Books have a title. Some books are considered "branch courses" of main texts.
- *unit* - part of a book; books have 2 units. Titled. Each unit has a purpose and a number of sections.
- *section* - a part of a study circle unit. Numbered. Untitled.

## Appendices

### Current Study Circle Books

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

- Book 3 BR1 -
- Book 3 BR2 - 
- Book 5 BR1 - Initial Impulse
- Book 5 BR2 - Widening Circle
- Book 7 BR1 - 

### Current Children's Classes Grades

- Grade 1
- Grade 2
- Grade 3
- Grade 4
- Grade 5

### Current Junior Youth Texts

- Breezes of Confirmation
- Wellspring of Joy
- Habits of an Orderly Mind
- Glimmerings of Hope
- Walking the Straight Path
- On Health and Well-Being
- Learning About Excellence
- Drawing on the Power of the Word
- Thinking About Numbers
- Observation and Insight
- The Human Temple
- Making Sense of Data 
- Spirit of Faith
- Power of the Holy Spirit
- Rays of Light 