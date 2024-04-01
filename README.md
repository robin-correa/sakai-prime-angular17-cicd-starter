# Sakai

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.0.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

## AWS S3 Static - Bucket Policy JSON

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::<bucket-name>",
                "arn:aws:s3:::<bucket-name>/*"
            ]
        }
    ]
}
```

## AWS CodeBuild - Environment Variable

- Add environment variable: `S3_BUCKET` and set the value to your corresponding bucket name.
  - Sample:
    - Key: `S3_BUCKET`
    - Value: `s3://angular17-cicd`
    - Type: `PLAINTEXT`

- Buildspec file:

```yml
version: 0.2

phases:
  install:
    commands:
      - echo "Installing dependencies..."
      - npm install

  build:
    commands:
      - echo "Building Sakai Prime Angular app..."
      - npm run build
      
  post_build:
    commands:
      - echo "Uploading / Syncing to S3"
      - aws s3 sync ./dist/sakai-ng $S3_BUCKET
```

## AWS CodePipeline

- Add `Build` stage
  - Set `Trigger` to `no filter`.
- Skip `Deploy` stage
- Save changes