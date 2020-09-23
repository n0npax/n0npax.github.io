# Readiness

Sometime you are making a money by a game where someone is trying to grow berries. If there is an outage for 48h you gonna loose profit. Nobody dies, nobody will sue you. If you are working on critical systems, you may need to pay a big fine in case of an outage. Usually delivering stable software later is worth more than bringing unstable one earlier. Customers will remember failure for long time as we all remember when AWS in north America was down. Don't we?

Quite often we need to explain to managers and even engineers that fact app is working on someones local machine or even in docker container doesn't mean it's production ready. Once you are in this situation, you may want to provide full list of what you are expecting for "production ready" application.

## What to check, what to copy/paste to your intranet

### Before discussing what readiness means for a system, lets chat if all critical decisions were done.

#### Starting project

* Do you understand traffic patterns?
  * Did you pick right interface? Graphql, grpc, rest?
* Which programming language you need want to use and why?
  * C/Go is faster in runtime, but development process in kotlin/python is faster.
* Avoid overcomplicated solutions. If something can be easy, make it easy.

#### Any project

* Is capacity estimation done? How many customers do you expect? Are those data up to date?
* Will dependencies scale up with your app? Will Service `bar` used by your component scale if required?
* What if your application will succeed? Is app able to scale up with customers? 
* How many single point of failures system has? Can `LB` mitigate an issue.
  * Live time of a single `HDD` in DC is around 3year. How old is one used by your node?
* Does the team understands domain boundaries
* Everyone in tech-team understands system overview and is able to debug
* Everyone understands system is making money when it is working.
  * Delivering unstable systems may impact income.
    * Ask bussines about lost revenue
    * ask tech about overtime to fix environment

## What to check when talking about readiness?

### Application

* Fallbacks are in place. If your app can fallback, do it.
* You store and read data in proper way.
  * IO is time expensive
  * Bad designed DB queries are slow
* You won't fail if not critical dependency is down.
  * If redis which gives you caching is down, you can still use fallbacks. You can also use local cache if redis is down.
* Logs are descriptive and useful. Understanding logs does NOT assume deep project knowledge.
 

### Development process

* Cycle is known and agreed. Everyone know that commit has to be reviewed.
* You have automated test
  * units
  * e2e
  * integration
  * Run performance tests as often as you can.
    * It's better to know performance regression was introduced by one of the commits on 5th of May instead of between March and July.
    * Compare max capacity from tests and one from design stage. Is it still safe?
* Production and final tests are done on version `X.Y.Z` not `latest`
* You are running linters
* You are running security scanning
* You are building and storing immutable artefact
  * You are using same versioning strategy everywhere. Consider `semver` whenever possible.
  * You building easy to deploy/install artefact like `docker` image or `snap` package.
  * You production build was made with optimization enabled and without enabled dev stacktraces
  * Building artefact is part of `CI/CD` process. You don't want to see issue with deprecated library just before deployment date
* After every commit/tag deploy your app to temporary environment and run tests in real environment. Once it's done remove resources. Once your team is bigger than 4-5 engineers it's better to check code before merging to the master.
* Treat code review as something important. This is not tick off process. It's to find issue with craftsmanship early. It's cheaper and faster to fix small piece of code today than dealing with legacy refactor in 3 years time.
  * Ensure engineers in the team understands value of code review.
* Consider chaos engineering, as it's a good gauge what have to be fixed/improved.

### Deployment

* Your deployment is fully automated. You may need to only click deploy and select a version. Nothing more 
* Something can always go wrong. You need way to rollback changes (this is one of the reasons why you cannot use `latest` in production). Treat this as an insurance. Once you can deploy any version of application via Your pipeline, you may need just a `db` migration schema
* Use canary deployment as preferred deployment
  * if not possible use Blue/Green deployment
  * if not possible, highlighter
* Keep at leas few environments and deploy to them one by one.
  * Before promoting to production, deploy to pre-prod. Monitor pre-prod and treat it as prod. If something in broken here, It'll be broken in production.
  * If you can, consider having `prod-mirror` environment. You can use it to test hot-fix before changing production.
* All dependencies are known and stored in single place.

### Environment

* Consider service discovery. It's overkill for startup, but must have for mid-size companies.
* Do you need?
  * Caching
  * Backups
* context of call and flow is shared between components
  * Corelation ID passed from one service to another
* Consider single platform to keep all apps
  * You can use service mesh to manage many apps in centralized way
    * Pros: centralized management, detailed flow data, enhanced security, simplified application development
    * Cons: additional complexity
  * You can hire platform team to provide shared goodies & re-use setup

### Monitor & Alert

* Does customers using ap 24h/day or rather 9-5?
* Monitor network stability
* APM - Always. Issue on productio may be hard to reproduce. APM gives you better overview.
  * monitor transactions
  * monitor resources
  * monitor calls
* collect quality and quantity data
* send logs to centralized system
* look for bottlenecks. Investigate - don't guess.
* Failure detections has to be automated.
  * Don't even thing about sending alert all the time. If issue is temporal, wait will working hours
  * If issue has no big impact, don't notify in the night
  * If issue is important, send notification.
    * If issue is not acknowledged for some time period, send escalation to next person in list automaticly.
  * There is hard requirement to have run-book with information:
    * How to fix known issues
    * Where escalate issues (who own downstream and list of senior engineers)
  * High alerts are send only if operator can perform any action.
  * Alert should contain link to documentation or at least uniq ID. At `3 AM` in night it's better to check instruction for handling issue `BOREW-07` instead of performing full analyze.
* There are at least few kinds of checks in production environment:
  * synthetics (can I click on my website. JS can be broken once backend is up)
  * api calls (is response as expected)
  * metrics based (Percentage of errors, load size)
  * logs based (regexp on logs)
  * healthz endpoints are fluctuating.
    * If single replica encouraged an issue and was recreated it doesn't mean you need to raise an alert. Details were already logged.
* Healthcheck is must have.
  * they should have 3 levels:
    * OK
    * WARN (do not eject or recreate)
    * ERROR
  * Except error code, response body should contain useful data. In example
  ```
    status: WARN
    code: 300
    downstream:
        critical:
            postgres: ok
        not_critical:
            minio: ok
            redis: down
    ```
* There are dashboards and engineers can see metrics for last few weeks.
    * All key metrics are known
    * Product/Customer related metrics are also known and stored  
* Logs are useful and descriptive.
    * Logs can be correlated  
* Traffic in your application is monitored.
  * If for some reasons 1 week ago traffic was around 70Mb/s and now is around 0.5Mb/s this may be an canary.

### Before armageddon starts - Disaster plan

* Scenarios have been identified. You have at leas an idea what can be done.
* Update procedures and run-book at least once per year. You can ask what was wrong with run-book on retro meeting and fix it/make tasks.
