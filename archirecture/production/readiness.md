# Readiness

Such is live as Aussies says. Quite often we need to explain to managers and engineer that fact app is working on someones local machine or even in docker container doesn't mean it's production ready.

Sometime you are making a money by a game where someone is trying to grow berries. If there is an outage for 48h you gonna just loose some profit. Noone dies, noone gonna sue you. If you are working on critical systems, you may need to pay a big fine in case of an outage. Usually delivering stable software later is worth more than bringing unstable piece of sh... earlier. We all remember when AWS in norh America was don't. Don't we?

## What to check, what to copy/paste to company intranet

### Before first line of code

* Think about a capacity. How many customers do you expect?
* Think what you need. Avoid overcomplicated solutions. If something can be easy, make it easy.
* Will dependency scale up with your app? Will Service `bar` used by your component scale up if required?
* Will your app scale up with customers? What if your application will succeed? 
* Do you understand traffic patterns?
  * Did you pick right interface? Graphql, grpc, rest?
* Do you need highly performant programming language?
  * Remember C/Go is faster in runtime, but development process in kotlin/python is faster.
* Your system has no single point of failure. Always scale & distribute. Use at least `LB`. Live time of a single `HDD` in DC is around 3year. How old is one used by your node?
* You discussed with team the domain boundaries
* Everyone in team understands system overview
* Everyone understands system is making money when it is working.

### Application

* fallbacks. If your app can fallback, do it.
* You store and read data in proper way.
  * IO is time expensive
  * Bad designed DB queries are slow
* You won't fail if not critical dependency is down.
  * If redis which gives you caching is down, you can still use fallbacks. You can also use local cache if redis is down.
* logs are descriptive and useful
 

### Development process

* cycle is known and agreed
* you have automated test
  * units
  * e2e
  * integration
  * run performance tests quite often.
    * It's better to know performance regression was intruduced by one of 5th of May instead of between March and July.
    * Compare max capacity from tests and one from design stage. Is it still safe?
* production and final tests are done on version `X.Y.Z` not `latest`
* you are running linters
* you are running security scanning
* you are building and storing immutable artefacts
  * You are using same versioning strategy everywhere. Consider `semver` whenever possible.
  * You building easy to deploy/install artefact like `docker` image or `dep` package.
  * You production build was made with optimalization enabled and without development stacktraces on the screen
  * building artefact is part of `CI/CD` process. You don't want to see issue with deprecated liblary just before deployment date
* after every commit/tag deploy your app to temporary environment and run tests in real environment. Once it's done remove resources. Once your team is bigger than 4-5 engineers it's batter to check code before merging to the master.
* Treat code review as something important. This is not tick off proces. It's to find issue with craftmentship early. It's cheaper and faster to fix small piece of code today than dealing with legacy refactor in 3 years time.
* Consider chaos engineering. It doesn't have to be a hard requirement, but it's a gauge what have to be fixed/improved.

### Deployment

* Your deployment is fully automated. You may need to only click deploy and select a version. Nothing more 
* Something can always go wrong. You need way to rollback changes (this is one of the reasons why you cannot use `latest` in production). Treat this as an insurance. Once you can deploy any version of application via Your pipeline, you may need just a `db` migration schema
* Use canary deployment as preffered deployment
  * if not possible use Blue/Green deployment
* Keep at leas few environments and deploy to them one by one.
  * Before promoting to production, deploy to pre-prod. Monitor pre-prod and treat it as prod. If something in broken here, It'll be broken in production.
  * If you can, consider having `production-mirror` to have a mirror to test hotfix on mirror before changing production.
* All dependencies are known and stored in single place.

### Environemnt

* Consider service discovery. It's overkill for startup, but must have for mid-size companies.
* There are shared solutions you can utilze. in example:
  * caching
  * backups
* context of call and flow is shared between components
  * There is request corellation ID passed from one service to another
* Consider single platform to keep all apps
  * You can use service mesh to manage many apps in centralized way
    * Pros: centralized management, detailed flow data, enhanced security, simplified application development
    * Cons: additional complexity
  * You can reuse some setup
  * You can keep single platform team to provide shared goodies.

### Monitor & Alert

* you know clients
* Monitor network stablity
* APM. Always
  * monitor transactions
  * monitor resources
  * monitor calls
* collect quality and quantity data
* send logs to centralized system
* If performance is required check if software is hardware friendly (check heap calls)
* look for bootelnecks. Investigate - don't guess.
* Faulure detections has to be automated.
  * Don't even thing about sending alert all the time. If issue is empheral wait will working hours
  * If issue has no big impact, don't notify in the night
  * If issue is important, send notification.
    * If issue is not acknowledged for some timeperiod, send notification to next persion in list
  * There is hard requirement to run-book with information who to notify when, as an engineer may not be aware who exactly owns service discovery component in company.
  * High alerts are send only is oprtator can perform any action.
  * Alert should contain link to documentation or at least uniq ID. At 3 AM in night it's better to check instruction for Issue `foo-07` than performing full analyze.
* There are at least few kinds of checks in production environemnt:
  * syntetics (can I click on my website)
  * api calls (is response as expected)
  * metrics based (in example percentage of errors, traffic size)
  * logs based (regexp on logs)
  * healthz endpoints are fluctuating.
* There are dasboards to see metrics for last few weeks.
* logs are useful and descriptive
* Traffic in your application to be aware of upcoming issues.

### Before armagedon starts

* Scenarios have been identified. You have at leas an idea what can be done.
* Update procedures and run-book frequently. You can ask what was wrong with run-book on retro meeting and fix it/make tasks.





