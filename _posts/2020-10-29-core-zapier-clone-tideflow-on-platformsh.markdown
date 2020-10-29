---
title: 'Core Zapier Clone: TideFlow. CI and Live on Platform.sh in 5 minutes - With
  Travis-CI'
date: 2020-10-29 20:35:00 +08:00
---

## Opinions expressed are solely my own

As a developer that having some exposure of pre-Cloud era, until currently cloud subscription based \+ Pay Per Use economy, I had seem the economy of the Cloud's Technology evolved like Telco industry did 10 years ago.

Zapier, one of the coolest automation tools in the market, I am not here to tell your whether the Zapier's economy is right or not. It surely worked for certain business, if you able to grab and compete with competition, or niche to hold your market. Either way, Open source's edition would mostly helped, at least when truly transparent processing, and "Data never gone out your infrastructure kind of operation". So you can integrate data outside of your infrastructure, and send them inside to your Cloud for processing.

For list of open source code workflow automation tools such as node-red, Flogo, node-red, etc. For the review of all of them, it can be valid for another blog post. I had choose to tested Tideflow, as it having super minimal proof of concept to get into it, and template. Potentially understand the downside of small project comparable to big project like node-red, and Flogo.

## Why I am choosing hosting it via Platform.sh vs Heroku?

Pro:

* Platform.sh is PaaS, similar to Heroku, stay focus on Stack rather than Linux Administrator, if you deep dive into feature building

* Platform.sh is Bandwidth unlimited. (Mostly 2nd highest in the list)

* Platform.sh included Support on compliance.

* Platform.sh having automatic SSL

* Having Travis-CI like with their Continuous deployment included, make your workflow only involved Dev and Ops

* Platform.sh 's Service.yaml is awesome than Heroku's addon like Redis and Postgres.

* Platform.sh dedicated having three-Virtual Machine redundant configuration provisioned support, when your business get success and prospect.

Cons:

* Platform.sh do not have buildpacks.io of Standard. but it having Travis-CI like of Building and Deploy step.

* You may not have IaaS like amazon, digital ocean

### Step 1:  Setup TideFlow in Travis-CI   (This step is Optional when I create other post, to run and build tideflow on Platform.sh)

1. Clone of https://github.com/tideflow-io/tideflow

2. Write your .travis.yml file like https://github.com/edwardchanjw/tideflow/blob/master/.travis.yml

3. You got the Travis-CI's build version of of Tar.GZ file

### Step 2.  Writing Platform.sh's Continuous deployment's YAML

1. Clone https://github.com/platformsh-examples/platformsh-example-meteor

2. Copied the Travis-CI's build version of of Tar.GZ file and also add the infrastructure as code: > .platform.app.yaml in Step 3.

3. .platform.app.yaml

```
# The name of this application, which must be unique within a project.
name: 'app'

# The type key specifies the language and version for your application.
type: "nodejs:12"

# The relationships of the application with services or other applications.
# The left-hand side is the name of the relationship as it will be exposed
# to the application in the PLATFORM_RELATIONSHIPS variable. The right-hand
# side is in the form `<service name>:<endpoint name>`.
relationships:
    database: "mydatabase:mongodb"

# The hooks that will be triggered when the package is deployed.
hooks:
    # Build hooks can modify the application files on disk but not access any services like databases.
    build: |
      export DB_USER="$(echo "$PLATFORM_RELATIONSHIPS" | base64 --decode | jq -r '.database[0].username')"
      export DB_PORT="$(echo "$PLATFORM_RELATIONSHIPS" | base64 --decode | jq -r '.database[0].port')"
      export MONGO_URL="mongodb://$DBUSER:$password@database.internal:$PORT"
      cd ./build
      tar -xf tideflow.tar.gz
      cd bundle
      (cd ./programs/server && npm install)
    # Deploy hooks can access services but the file system is now read-only.
    deploy: |


# The size of the persistent disk of the application (in MB).
disk: 512

# The 'mounts' describe writable, persistent filesystem mounts in the application.
# The keys are directory paths relative to the application root. The values are
# strings such as 'shared:files/NAME' where NAME is just a unique name for the mount.
mounts:
  "/run": "shared:files/run"

# The configuration of the application when it is exposed to the web.
web:
    commands:
      start: |
        export DB_HOST=`echo $PLATFORM_RELATIONSHIPS|base64 -d|jq -r ".database[0].host"`
        export DB_PASS=`echo $PLATFORM_RELATIONSHIPS|base64 -d|jq -r ".database[0].password"`
        export DB_USER=`echo $PLATFORM_RELATIONSHIPS|base64 -d|jq -r ".database[0].username"`
        export DB_PATH=`echo $PLATFORM_RELATIONSHIPS|base64 -d|jq -r ".database[0].path"`
        export DB_PORT=`echo $PLATFORM_RELATIONSHIPS|base64 -d|jq -r ".database[0].port"`
        export MONGO_URL="mongodb://$DB_USER:$DB_PASS@$DB_HOST:$DB_PORT/$DB_PATH"
        node build/bundle/main.js
    locations:
      "/public":
        passthru: false
        root: "public"
        # Whether to allow files not matching a rule or not.
        allow: true
        rules:
          '\.png$':
            allow: true
            expires: -1

```

---

Then change the ROOT_URL to your Platform.sh's provided URL, probably I can update Step 3 later...because I don't have time to test it now.

I have the urge to learn some Next.JS first, then I am bringing you the updated version of this article and another how to setup Node.js boilerplate, NEXT.js / Nuxt continuous deployment on Heroku and Platform.sh in next week. If you having any question, please don't afraid to ask, but I might update this post to new method soon.

So that is, I am probably also setup my own subscription SaaS for some small market soon, but mostly I would write a boilerplate to share open source. See ya next time.