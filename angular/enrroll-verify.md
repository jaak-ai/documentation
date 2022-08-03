---
sidebar_position: 3
---
# Enroll Verify

Jaak Enroll© Component is an isolated component of Angular that is easy to integrate that allows you to verify a user with facial recognition.

## Features

- Get permission to access the camera
- Video Capture modal
- Verify Capture Component

Jaak Enroll Verify© Component is an isolated component of Angular that is easy to integrate that allows you to verify the registry a user with facial recognition, which was developed by JAAK-IT.

## Tech

Technologies used for the development of this library:

- Angular
- Scss
- Typescript
- Rxjs
- Angular Material

## Installation

Jaakrecog Enroll requires [Node.js](https://nodejs.org/) v10+ to run.

Install the package:

```sh
npm install jeekrecog-verify
```

## Setup
Update Assets configuration on angular.json file. Just add this line to assets array: 
```sh 
      { "glob": "**/*", "input": "./node_modules/jaakrecog-verify/assets", "output": "/assets" } 
```
This is an example:

```sh 
{
    "architect":{
        ...
        "options": {
            ...

            "assets":[
                ... ,
                { "glob": "**/*", "input": "./node_modules/jaakrecog-verify/assets", "output": "/assets" }
            ]
        }
    }
}

```
Import VerifyModule inside app.module.ts file and add the api path that you are goig to use (optional) the value as default is: 'https://dev-facade-1ton-http-635t26xtnq-uc.a.run.app/api/v1/one2n/enroll/'.

```sh
    VerifyModule.forRoot({
      apiUrl: 'https://dev-facade-1ton-http-635t26xtnq-uc.a.run.app/api/v1/one2n/verify/',
    }),
```

## Usage


Use the selector inside of your html file to use the component, the input uses is a require.

```
<jaakrecog-verify (getResponse)="yourFunction($event)"></jaakrecog-verify>

```

OUTPUT
To get the response of the process you have to use the output (getResponse) inside of component tag.

The response of this promise would be a class named OneToNVerify

```sh
export class OneToNVerify {
  id!: string;
  status!: boolean;
  enrolled!: boolean;
  error!: string;
  processingTime !: number;
  user!: {
    id: string;
    name?: string;
    firstSurname?: string;
    secondSurname?: string;
    email?: string;
    status: string;
  };
}
```

We can get an error and it will return an error of type ErrorResponse

```
 class ErrorResponse {
 type!: string;
 message!: string;
 }
```

