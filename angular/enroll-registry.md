---
sidebar_position: 2
---

# Enroll Registry

Jaak Enroll© Component is an isolated component of Angular that is easy to integrate that allows you to registry a user with facial recognition.

## Features

- Get permission to access the camera
- Video Capture modal
- Enroll Capture Component


## Tech

Technologies used for the development of this library:

- Angular
- Scss
- Typescript
- Rxjs
- Angular Material

## Installation

Jaak Enroll requires [Node.js](https://nodejs.org/) v10+ to run.

Install the package:

```sh
npm install enroll-jaakrecog
```

## Setup
Update Assets configuration on angular.json file. Just add this line to assets array: 
```sh 
      { "glob": "**/*", "input": "./node_modules/enroll-jaakrecog/assets", "output": "/assets" } 
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
                { "glob": "**/*", "input": "./node_modules/enroll-jaakrecog/assets", "output": "/assets" }
            ]
        }
    }
}

```
Import EnrollModule inside app.module.ts file and add the api path that you are goig to use (optional) the value as default is: 'https://dev-facade-1ton-http-635t26xtnq-uc.a.run.app/api/v1/one2n/enroll/'.

```sh
    EnrollModule.forRoot({
      apiUrl: 'https://dev-facade-1ton-http-635t26xtnq-uc.a.run.app/api/v1/one2n/enroll/',
    }),
```

## Usage

INPUTS

```sh
userData: Object<any>;
accessToken: string;
```
use userData input to pass the info to registry, for example:

```sh
userData = {
    name: string,
    email: string,
};
```
use accessToken to send request to the JAAKRECOG server


OUTPUT
To get the response of the process you have to use the output (getResponse) inside of component tag.

The response of this promise would be a class named OneToNEnroll

```sh
class OneToNEnroll {
    id!: string;
    status!: boolean;
    enrolling!: boolean;
    message!: string;
    error!: string;
    processTime!: number;
}
```

We can get an error and it will return an error of type ErrorResponse

```
 class ErrorResponse {
 type!: string;
 message!: string;
 }
```

Use the selector inside of your html file to use the component, the input uses is a require.

```
<jaakrecog-enroll [accessToken]="your_TOKEN" [userData]="your_object_user" (getResponse)="yourFunction($event)"></jaakrecog-enroll>

```