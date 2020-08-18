# How to report an issue like a pro

Reporting issues seems to be very easy task, but in real live it's not.

Asking for help too often or in wrong way may cause rejection and de-prioritization. At the end of the day sympathy and respect are important factor in any company.

## Why reporting issue is hard

Because there is a need to provide **useful information** in **useful form** to people who can & **should** help. You see those 3 bolded parts. Let's break them down.

### Useful information

Issue report like:

> Application foo is broken

is basically useless. Person who will try to help, will need to check and ask why it is broken, since when and in which environment. Basically information is too generic.

So what is useful information? As always, it depends... In small company where there is 20 engineers there is no need to provide team name, once in enterprise this kind of data may be useful.

Never the less, usually set of detail may contain:

> What application/service: Service `Foo`

> What is broken: http `HEAD` request causes response 500 from server

> How it happened: We tried to connect service `Bar` to `Foo`

> What environment: Testing

> How to reproduce: `curl -I http://foo.svc.cluster.local`

> Link to the centralized logs: https://my.elk.example.com/1234

If you can perform any analysis, just do it. Add note on all actions you have done.

> Actions taken: We checked this, this and that.

### Useful form

Useful form means logs/description easy to read and use. Once you are trying to say burger on the website is weirdly shifted to the left attaching piece of sceenshot is just ok.

[![alt text](../img/burger.png "")]

But sharing logs as an image is just **wrong**. Do you really want to loose someones time to rewrite `request id` or `x-3b` header to perform analysis and help you?

**Always** provide logs and data as a text which is **readable**.

When you are asking someone to read this:

> 14:27:25.426968 IP (tos 0x0, ttl 62, id 53988, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.1.24.59238 > 93.184.216.34.80: Flags [.], cksum 0x749a (correct), ack 1, win 502, options [nop,nop,TS val 1591297662 ecr 20350599], length 0
14:27:25.429880 IP (tos 0x0, ttl 62, id 53989, offset 0, flags [DF], proto TCP (6), length 298)
    192.168.1.24.59238 > 93.184.216.34.80: Flags [P.], cksum 0xa18f (correct), seq 1:247, ack 1, win 502, options [nop,nop,TS val 1591297664 ecr 20350599], length 246: HTTP, length: 246
        GET /default HTTP/1.1
        Host: 192.168.39.206
        Remote-Addr: 127.0.0.1
        Enriched: true
        X-Real-Ip: 172.17.0.1
        Connection: close
        User-Agent: curl/7.65.3
        Accept: */*
        Environment: dev
        Host: example.com
        dispatched: True
        Accept-Encoding: gzip, deflate
14:27:25.637488 IP (tos 0x0, ttl 57, id 26254, offset 0, flags [none], proto TCP (6), length 52)ui' as ui show Paint, Path, Canvas;

You are showing just disrespect to person you are trying to work with.

Once you will use proper formatting, reading logs became easier. This is crucial as converting logs back to readable form is manual task which is time consuming. Instead of loosing 5 minute of your partner you can spend additional 5 seconds. 5s/300s your time is not worth 60 000 someones time, sorry.

```
14:27:25.426968 IP (tos 0x0, ttl 62, id 53988, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.1.24.59238 > 93.184.216.34.80: Flags [.], cksum 0x749a (correct), ack 1, win 502, options [nop,nop,TS val 1591297662 ecr 20350599], length 0
14:27:25.429880 IP (tos 0x0, ttl 62, id 53989, offset 0, flags [DF], proto TCP (6), length 298)
    192.168.1.24.59238 > 93.184.216.34.80: Flags [P.], cksum 0xa18f (correct), seq 1:247, ack 1, win 502, options [nop,nop,TS val 1591297664 ecr 20350599], length 246: HTTP, length: 246
        GET /default HTTP/1.1
        Host: 192.168.39.206
        Remote-Addr: 127.0.0.1
        Enriched: true
        X-Real-Ip: 172.17.0.1
        Connection: close
        User-Agent: curl/7.65.3
        Accept: */*
        Environment: dev
        Host: example.com
        dispatched: True
        Accept-Encoding: gzip, deflate

14:27:25.637488 IP (tos 0x0, ttl 57, id 26254, offset 0, flags [none], proto TCP (6), length 52)ui' as ui show Paint, Path, Canvas;
       ^
```

## Should this issue be solved by someone else?

Sometimes it's obvious, You are engineer in project `A` and project `B` is sending wrong request. Report, explain problem and let them work.

Unfortunately this isn't the case all the times. Quite often there is a middle ground.

Issue may be somewhere between. In this case it's also your problem, you can't just accuse project and wait for it to be fixed. You need to cooperate. All the time try to explain what was done to check this on your side. Provide output and ways how you got those.

In example answer on question about my machine IP can be:
```
hostname -I
10.10.3.3 10.10.42.42
```
Let's say, I'm going to share this with network engineer. So what is benefit? He is sure how IP was acquired and in case he believe I gave him right data. Unfortunately it's quite common people are sharing data they believe are right, but data are acquired in wrong way. By adding command on the top, we can help to find those issues as early as possible.

### Build is broken, bloody scm/devops team

Have you check why it's failing? What **logs** are telling you? Is it due to unstable jenkins or maybe your tests are broken.

Lets easily check if jenkins failing due to `OOM`? Is this expected that your application is consuming 22Gi of memory? Is this `OOM` kill unfair? **Always** check before asking for help/escalation. 

Usually actions like build, tests are done in linux container. Have you tried to run this on your machine? this will exclude any difference between your local development machine and CI. I believe you, that tests all all green on your MAC, but maybe Makefile used to build is missing installation of newest mesa driver? Once you report contains confirmation that build/tests were executed successfully in docker, but same build fails in CI, this may be your shot. But remember **Always** check before asking for help/escalation. 

### Server is responding with the error

Remember, the fact you are expecting server to behave in particular way doesn't mean it will. Business logic can change over the time and new fields can be added, once other can be deprecated. Fact you are facing error response may mean you have not adjusted to new interface.
Usually error means error, but remember, this is no always the case.

### Platform

Sometimes team of engineers is responsible for whole `SDLC`, sometime some work is outsourced to central team. In case your application is running on server you don't own, you will be provided access to the logs. You can't modify server, but you can check what is going on. If development/testing environment is down, it's time for you to check. Did I say **Always** check before asking for help/escalation? You application can help due to issue with business logic or health checks. You have written the code to check if app is able to handle the traffic. Logs are says DB is down. This smells like a request to DB team. You just skipped one stage and whole engagement process should be faster. Platform/system is ok, as it keeps you app up, but app is not able to handle traffic due to DB issue.

### Don't notify everyone

If you application is down, is there a need to notify whole technical staff? Usually single team is enough. If you are using `irc`,`slack`,`teams` or whatever you use, do you really want to notify everyone in the channel? if this is 20 folks, ok... But what if there is 500-1000 people in the channel. Do you really want to notify 500 members of cloud channel in case you app is down. Those people are from random teams, you just caused context switching for 0.5K people.

### Summary

Small number of requests and accurate descriptions causes people are more helpful.
Once your previous requests were descriptive and shown you tried to solve issue, people will just give you extra points.

Once there is another request raised by a guy who is not reading the logs and asking someone to find an issue for him, he will be prioritized. Once this kind of request arrives people may just decide to get lunch or continue with task. Even if issue is important sympathy and irritation are important factors. Even if management will provide other directions, humans are humans.

Don't shout on communicators(`@here`) unless you really want to notify everyone.