# Software quality
This file containes all the proof for the Software quality learing outcome.
> **Tooling and methodology:** Carry out, monitor and report on unit integration, regression and system tests, with attention for security and performance aspects, as well as applying static code analysis and code reviews.

## Sonarcloud
For both the group project and the individual project we use sonarcloud to review our code quality and coverage on our tests. 

Here are the links to the Sonarcloud organisations:
- [Sonarcloud group project](https://sonarcloud.io/organizations/modus-1/projects)
- [Sonarcloud individual project](https://sonarcloud.io/organizations/ips3-db04-teun-mos-lukas-jansen/projects)

### Quality gate conditions
For a Sonarcloud quality gate to pass you need to set some conditions first.

These are the conditions for our **group project**'s quality gate to ***fail***:
|Metric|Operator|Value|
|--|--|--|
|Coverage|is less than|80.0%|
|Duplicated Lines (%)|is greater than|3.0%|
|Maintainability Rating|is worse than|A|
|Reliability Rating|is worse than|A|
|Security Hotspots Reviewed|is less than|100%|
|Security Rating|is worse than|A|
> this info may be out of date,  to ensure you have the latest information [*click here!*](https://sonarcloud.io/organizations/modus-1/quality_gates/show/9)

## Group project

### branches
For our group project we protected the "main" and "development" branches on our repositories. This means you need to create a pull request to merge into those branches.
For our group we have made it so you have to meet a few requirements before a pull request can be approved and merged:
- The pull request must be approved by at least 2 other group members.
- The code must have a succesful build.
- The Sonarcloud quality check must pass.

### Unit tests
We created a lot of tests for our repositories in the group project, but the tests i've made myself *(together with [Lukas](https://github.com/LukasJansen100))* are mainly in the menu-api repository which you an check out [*over here!*](https://github.com/Modus-1/menu-api)

## Individual project
### branches
Just as we did for our group project, our individual project has the "main" branch protected, so you need to make a pull request to merge into those branches.
For our individual project we also made a few requirements before a pull request can be approved and merged, because i'm doing the project with [Lukas](https://github.com/LukasJansen100) we can't make it so 2 people need to approve a pull request. 
but apart from that the requirements before a pull request can be approved are the same:
- The pull request must be approved by the other member.
- The code must have a succesful build.
- The Sonarcloud quality check must pass **(W.I.P.)**.

