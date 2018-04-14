+++
date = "2017-04-18T21:11:22-04:00"
description = ""
draft = false
slug = "google-cloud-setup-golang"
tags = []
title = "Google Cloud Platform Setup: Golang"
topics = []

+++

## Install Google Cloud Tools

1. [Download](https://cloud.google.com/appengine/docs/standard/go/download) and Install the SDK for Google App Engine for Golang. Select the Mac OS X x86_64 package.
2. Extract the file into the home path `~`.
3. Run the install script.

```
./google-cloud-sdk/install.sh
```

1. Run `gcloud init` to initialize the SDK

```
./google-cloud-sdk/bin/gcloud init
```

## Google Cloud Components

The default installation does not include the App Engine extensions required to deploy an application using `gcloud` commands. These components can be installed using the [Cloud SDK component manager](https://cloud.google.com/sdk/docs/managing-components).

To see which components are available and which are currently installed, run

```
gcloud components list
```

![Google Cloud SDK](/posts/2017/0418-google-cloud-setup-golang/google-cloud-sdk.png)

Note the general usage notes:

```
To install or remove components at your current SDK version [151.0.1], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [151.0.1], run:
  $ gcloud components update
```

Install the following components:

* app-engine-go
* app-engine-python
* cloud-datastore-emulator

```
gcloud components install app-engine-go
gcloud components install app-engine-python
gcloud components install cloud-datastore-emulator
```

## Google Cloud Client Libraries

The [Google Cloud Client Libraries](https://cloud.google.com/apis/docs/cloud-client-libraries) are the recommended method for calling Google Cloud APIs. To install the golang version 

```
go get -u cloud.google.com/go/...
```

Reference the Google Cloud package [docs](https://godoc.org/cloud.google.com/go).
