# Before making any big decisions let's consider given topics:

Let's check and mark every single point here. Some characteristics may not apply, as stateless application may not need to backup any data. In this case answer can be `N/A`. Some of features are critical, once other are not, but all of them should be considered and kept as part of design. The priority always comes from problem domain. You cannot satisfy every characteristic, but there should be an information what was prioritized and why. Final list should be short whenever possible.



| characteristic | comments | answer|
|---|---|---| 
| Corectness |What can we do, to ensure system will work as expected |
| Learnability  | Engineers are not cheap. How long will it take to learn systems and create mental model for new-hire? |
| Agility   | How hard it will be to change scope of work ||
| Testability | How can we proof system works as expected, How can we proof `e2e` is checking proper domains |
| Elasticity | Will traffic be on similar level, or spikes and drops are expected?| |
| Deployability |Will deployment process reproducible and easy | |
| Development lifecycle | Is process of building new puzzles known and structured? ||
| Performance | Do you care about performance? What is traffic estimation? | |
| Scalability  | How will you and your dependency will scale | |
| Availability   | 24/7 or maybe 7-19 ? ||
| Reliability | What level of?, how critical is it? ||
| Robustness | what kinds of errors are expected? ||
| Continuity | DR capabilities ||
| Security | Will any bridge be a disaster for company and customers? ||
| Recoverability |How to recover system in case of environment failure? How fast does it has to happen?  How much data can be lost? ||
| Fault tolerance | Which kind of outage are we expecting ||
| Scalability | How scaling can be implemented? Is it required?||
| Auditability | Does system has to pass any audits?||
| Data   | What can we store? What can be processed (legal)||
| Configurability | what level of parametrization is required? ||
| Supported platforms (if required) | Do you need to support all operating systems? Does it apply? ||
| Maintability | How easy is to keep system up and running||
| Portability |How important is possibility of software to new env/hardware/OS? ||
| Supportability | What level of support for application will be required||
| Upgradebility |How single app and whole system handles upgrades? Is it required? Is multiversion support required?||
| Accessibility | what with folks with disabilities? ||
| Archieveability | Can we/do we need to archive or delete data (GDPR?) ||
| Legal requirements |||
| Privacy | can we hide transactions from internal employees? DDM? ||
| Compatibility | is compatibility with other components required? I  backward compatibility required?||
| Usability | How much do you care about users? Are there forced to use this system, or do you need to compete? ||
| functional suitability | what mean completeness/correctness/mvp for this app/system? || 