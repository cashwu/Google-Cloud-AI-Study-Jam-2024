
# Develop GenAI Apps with Gemini and Streamlit : Challenge Lab

> please use your `Project ID` and `Region`

## Task 1. Use cURL to test a prompt with the API

modify prompt.py

## Task 2. Write Streamlit framework and prompt Python code to complete chef.py

modify chef.py

## Task 3. Test the application

install package

```
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
pip install -r requirements.txt
```

set env

```
PROJECT='qwiklabs-gcp-01-cdea44ed5449'
REGION='us-central1'
```

run app

```
streamlit run chef.py \
--browser.serverAddress=localhost \
--server.enableCORS=false \
--server.enableXsrfProtection=false \
--server.port 8080
```

## Task 4. Modify the Dockerfile and push image to the Artifact Registry

dockerfile

```
ENTRYPOINT ["streamlit", "run", "chef.py", "--server.port=8080", "--server.address=0.0.0.0"]
```

build image

```
AR_REPO='chef-repo'
SERVICE_NAME='chef-streamlit-app'
gcloud artifacts repositories create "$AR_REPO" --location="$REGION" --repository-format=Docker
gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME"
```

## Task 5. Deploy the application to Cloud Run and test

deploy image to cloud run

```
gcloud run deploy "$SERVICE_NAME" \
--port=8080 \
--image="$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME" \
--allow-unauthenticated \
--region=$REGION \
--platform=managed  \
--project=$PROJECT \
--set-env-vars=PROJECT=$PROJECT,REGION=$REGION
```
