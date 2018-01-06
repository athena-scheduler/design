# Models

This document maps out some of the data types we might need.
The fields and data types outlined in this document do not
necessarily line up with how relationships in the database
will be structured. 

## Student

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the student |
| Name | String | The name of the student |
| Email | String | The email address of the student |
| Institutions | [`Institution[]`](#institution) | The institutions the student belongs to |
| Program | [`Program[]`](#program) | The programs the student is enrolled in. Most students will only be enrolled in one program at a time |
| CompletedCourses | [`Course[]`](#course) | The courses the student has already completed |


A `Student` is one of the top-most datatypes in our hierchy.
Students enroll in [`Program`](#program)s and may have taken
one or more [`Course`](#course)s already. Students belong to
one or more [`Institution`](#institution)s.

## Institution

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the institution |
| Name | String | The name of the institution |
| Description | String | A discription or motto of the institution |
| Campuses | [`Campus[]`](#campus) | The campuses on which the institution offers courses |
| Programs | [`Program[]`](#program) | The programs offered by the institution |
| Courses | [`Course[]`](#course) | The courses offered by the institution |

Institutions upload [`Course`](#course)s and [`Program`](#program)s
to the system for students to take. This contains information
about what [`Room`](#room)s and [`Campus`](#campus)es these are
offerred on to aid students in scheduling. In this way, a 
student can mark what [`Campus`](#campus) they prefer if a
[`Course`](#course) is offerred on multiple [`Campus`](#campus)es.

## Program

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the program |
| Name | String | The name of the program |
| Description | String | The description of the program |
| Institution | [`Institution`](#institution) | The institution that offers this program |
| Requirements | [`Requirement[]`](#requirement) | Courses that must be completed to count this program as completed |

A `Program` is roughly analogous to a Major. Each program
has one or more [`Requirement`](#requirement). Note that we do
not directly associate [`Course`](#course)s with [`Program`](#program)s
as different institutions may offer [`Course`](#course)s that
satisfy the same [`Requirement`](#requirement).

## Course

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the course |
| Name | String | The name of the course |
| Description | String | The description of the course |
| Institution | [`Institution`](#institution) | The institution that offers the course |
| SatisfiedRequirements | [`Requirement[]`](#requirement) | The requirements this course satisfies |
| Prerequisites | [`Requirement[]`](#requirement) | Any courses that must be completed before this course can be taken |
| ConcurrentPrerequisites | [`Requirement[]`](#requirement) | Any courses that are required to be taken before or concurrently with this course |
| Offerings | [`Offering[]`](#offering) | The dates and times the course is offerred |

A `Course` contains a list of [`Requirement`](#requirement)s
that it satisfies, as well as [`Requirement`](#requirement)s
for prerequisites. For some `Course`s, a [`Requirement`](#requirement)
might be able to be taken concurrently with the `Course`.
Each course has one or more section or [`Offering`](#offering),
which is what actually ends up on the calendar.

## Campus

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the campus |
| Name | String | The name of the campus |
| Description | String | A description of the campus |
| Location | String | The address of the campus |
| Institution | [`Institution[]`](#institution) | The institutions that offer courses on the campus |
| Rooms | [`Room[]`](#room) | The rooms on the campus |

[`Institution`](#institution)s may have multiple `Campus`es
that they offer [`Course`](#course)s on. This distinction is
important as [`Student`](#student)s may prefer to take
[`Course`](#course)s on a `Campus` that is closer to home.

## Requirement

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the requirement |
| Name | String | The name of the requirement |
| Description | String | A brief description of the requirement |

A simple abstraction around [`Course`](#course)s so students
can take [`Course`](#course)s from multiple different
[`Institution`](#institution)s towards completion of their
[`Program`](#program)s.

## Offering

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the offering |
| Course | [`Course`](#course) | The course this offering is for |
| Campus | [`Campus`](#campus) | The campus this offering is provided from |
| Start | Date | The day the offering starts |
| End | Date | The last day of the offering |
| Meetings | [`Meeting[]`](#meeting) | The days of the week this offering meets |

Given a starting and ending date, an `Offering` for a
[`Course`](#course), an `Offering` is simply a collection of
[`Meeting`](#meeting)s. If the `Offering` online only, no
[`Meeting`](#meeting)s are specified

## Meeting

| Field | Type | Description |
| ----- | ---- | ----------- |
| Id | Guid | An identifier that uniquely identifies the meeting |
| Course | [`Course`](#course) | The course that the meeting is for |
| Offering | [`Offering`](#offering) | The offering that the meeting is for |
| Day | DayOfWeek | The day of the week the meeting is on |
| Time | TimeOfDay | The time of day the meeting is at |
| Duration | Duration | The length of time the meeting lasts for |
| Room | [`Room`](#room) | The room the meeting is in |

A day during the week that a [`Course`](#course) meets for a
given [`Offering`](#offering).
