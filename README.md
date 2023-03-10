# angular-cloud-run-poc
POC of running an Angular app in GCP's Cloud Run

## Setting up Infrastructure

1. Create GCP project
2. Update tf/main.tf with your project ID.
3. Generate infrastructure
```
gcloud auth application-default login
terraform init
terraform apply
```

## Set up UI

1. Create new app from Angular CLI `ng new your-app`
2. Create `index.js`
3. Test with `npm ci && npm run build && node index.js`
4. Create Dockerfile

[Reference](https://medium.com/@larry_nguyen/how-to-deploy-angular-application-on-google-cloud-run-c6d472e07bd5)

## Set up API

1. Create WebAPI with `dotnet new webapi -o api`
2. Change WeatherForecastController route to ""
3. Create Dockerfile

[Reference](https://codelabs.developers.google.com/codelabs/cloud-run-hello-csharp#3)

## Test Docker Image

With Docker in docker installed and while CDed into the UI/API directory:

1. `docker build -t app .`
2. `docker run -p <YOUR_PORT>:<CONTAINER_PORT> app`

## Build & Deploy

_This is done separately for both UI and API_

1. CD into either UI or API.
2. `gcloud config set project PROJECT`
3. `gcloud auth login`
4. `gcloud builds submit --tag gcr.io/PROJECT/SERVICE`
5. `gcloud run deploy SERVICE --image gcr.io/PROJECT/SERVICE --region us-central1 --allow-unauthenticated`
6. Test using URL provided from deploy.

## Troubleshooting

If having an issue getting a image to start, try running it locally after pushing:

```
gcloud auth configure-docker
gcloud container images list
docker run -p 8080:8080 gcr.io/PROJECT/SERVICE
```

[Reference](https://medium.com/@larry_nguyen/how-to-deploy-angular-application-on-google-cloud-run-c6d472e07bd5)
