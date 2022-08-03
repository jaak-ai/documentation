---
sidebar_position: 1
---

# Liveness Best Frame

Component developed to capture the liveness service

## Features

- Get permission to access the camera
- Video Capture modal
- Liveness Bestframe Service

Jaakrecog Liveness© Component is an isolated component of Angular that is easy to integrate that allows you to capture the video process in order to make a liveness, which was developed by JAAK-IT.

## Tech

Technologies used for the development of this library:

- Angular
- Scss
- Typescript
- Rxjs
- Angular Material

## Installation

Jaakrecog Liveness requires [Node.js](https://nodejs.org/) v10+ to run.

Install the package:

```sh
npm install jaakrecog-liveness-lib
```

## Setup

Declare on import of JaakrecogLivenessLibModule module

```JaakrecogLivenessLibModule.forRoot({
      apiUrl: 'https://dev.api.jaakrecog.com',
    }),
```

## Usage

Uses the asynchronous function openVideoCapture found in the JaakrecogLivenessLibService service

```import { JaakrecogLivenessLibService } from 'jaakrecog-liveness-lib';```

```initCaptureVideo(): void {
         this.jaakrecogLivenessSrv.openVideoCapture('your_token_here')
           .then((data) => {
                   console.log(data);
               })
           .catch((error) => {
                   console.log(error);
             });
          }
```

The response of this promise would be a class named BestFrame

```
  class BestFrame {
    bestFrame!: string;
    evaluation!: number;
    facesFound!: Array<string>;
    id!: string;
    message!: string;
    processTime!: number;
    status!: boolean;
  }
```

We can get an error and it will return an error of type ErrorResponse

```
 class ErrorResponse {
 type!: string;
 message!: string;
 }
```

Another alternative of use, that we can implement is through the selectors, for this it uses in an HTML file

```
<ngx-capture-video [token]="your_TOKEN" [stream]="MediaStream" (getBestFrame)="yourFunction($event)"></ngx-capture-video>
```

This method of use requires obtaining the userMedia previously or leaving it null for the component to raise the request for camera permissions.

## Functions

Access to the library functions from the JaakrecogLivenessService

| Name             | Info                             |
| ---------------- | ---------------------------------- |
| getPermissions   | get access camera permissions      |
| openVideoCapture | open video capture modal component |
| verifyBestFrame  | request to the server              |
