---
author: content/data/nacho.json
colors: bg-light-fg-dark
date: '2023-07-10'
featuredImage:
  altText: Thumbnail
  styles:
    self:
      borderRadius: medium
  type: ImageBlock
  url: /images/time.jpg
isFeatured: false
seo:
  metaDescription: Resilient CronJob-based Webhook in K8S for Distributed services.
  metaTitle: Resilient CronJob-based Webhook in K8S for Distributed services
  socialImage: /images/time.jpg
  type: Seo
slug: k8s-cronjob
styles:
  self:
    flexDirection: col
title: Resilient CronJob-based Webhook in K8S for Distributed services
type: PostLayout
---

# Resilient CronJob-based Webhook in K8S for Distributed services

## Requirements

I needed a cronjob-based webhook with retries. In other words, if I had a service called 'X' with 10 replicas, I wanted a CronJob that would execute only once on any of those replicas. Additionally, I wanted to ensure that the cron job had resilient retry strategies in case of failures.

## 💡 Solution Implemented

We leveraged a CronJob in Kubernetes to address this requirement. We defined the desired schedule for the cron job, but how did we ensure it only ran on a single replica? The solution wasn't to rely on code-based scheduling because with our "10 replicas," the schedule would execute on each of them, which didn't serve our purpose.

Instead, we used a CURL image of type 'Webhook.' By doing this, we knew that the request executed by the CronJob would be load-balanced by Kubernetes. This approach not only enabled us to target a single replica but also provided built-in retry strategies if the CURL request (e.g., CURL -X POST) didn't return a 200 OK status code.

🚀 By combining Kubernete's powerful scheduling capabilities like  [cronJob](https://crontab.guru/?ref=0.0.0.0#10_*_*_*)  and the use of a CURL image, we achieved our goal of executing a distributed cron job with retry resilience. This solution offered the scalability and reliability we needed while leveraging the strengths of the Kubernetes ecosystem.

```bash
apiVersion: batch/v1
kind: CronJob
metadata:
  name: firsts-cronjob
spec:
  schedule: "00 00 1 * *" 
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: firsts-cronjob-container
              image: curlimages/curl
              resources:
                limits:
                  cpu: "1"
                  memory: "300Mi"
                requests:
                  cpu: "1"
                  memory: "300Mi"
              args:
                - /bin/sh
                - -c
                - |
                  date
                  echo "Starting CronJob"
                  resp=$(curl -X POST -I -s -o /dev/null -w "%{http_code}" --http2 http://service-x:80/webhook | cut -d$' ' -f2)
                  if [ "$resp" -ne 200 ]; then
                    echo "Error: failed with http: $resp" && exit 1
                  else
                    echo "Success: http: $resp" && exit 0
                  fi
          restartPolicy: OnFailure
  successfulJobsHistoryLimit: 12
  failedJobsHistoryLimit: 12
```

### Retries example

![](./images/k8s-cronjob.png)

HTTP != 200's

## **Common Problems**

Recently, I was working at this company where they had this hilarious issue with cron jobs. They were like, "Don't even think about using cron jobs with Kubernetes! We tried it once, and it was a disaster!" 😭🚫

So, guess what the genius "pseudo-IT" team did to handle distributed cron jobs? They created their own janky solution! They built a project from scratch to manage cron jobs, complete with database locks and tightly coupled code using the @Schedule annotation in Spring Boot. But here's the catch: no architecture, no documentation, and no best practices whatsoever!

Can you believe it? Instead of trusting a battle-tested and widely-used tool like Kubernetes, they went on an adventure to reinvent the wheel. And guess how they determined if their cron job was successful? Brace yourself... they didn't care about the actual HTTP response code! They just returned an exit 0, considering it a triumph every time. 🙃

```
              args:
                - /bin/sh
                - -c
                - |
                  date
                  echo "Starting card CronJob"
                  curl -X POST --http2 http://service-x:80/webhook 
                  exit 0
              restartPolicy: OnFailure
```

this is the reason why the company decided to spend 3 to 4 months developing a new project (how much theft)

## Conclusion

> Never underestimate the tried-and-true tools that have been battle-tested by the IT community. Don't be tempted to take shortcuts and reinvent the wheel when there's a perfectly good solution available. Embrace the tools that have proven their worth and save yourself from unnecessary headaches! 💡