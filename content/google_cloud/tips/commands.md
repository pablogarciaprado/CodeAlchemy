◀️ [Home](../../../README.md)

## Google Cloud Commands

### Google Cloud Storage
#### Copy a file to or from a Google Cloud Storage bucket.

`gcloud storage cp` copies files between local storage and Google Cloud Storage (GCS) or between GCS buckets. The basic syntax is:
```bash
gcloud storage cp [OPTIONS] source destination
```

##### Examples
1. Upload a file to GCS:
```bash
gcloud storage cp file.txt gs://my-bucket/
```
2. Download a file from GCS:
```bash
gcloud storage cp gs://my-bucket/file.txt .
```
3. Copy a file between GCS buckets:
```bash
gcloud storage cp gs://source-bucket/file.txt gs://destination-bucket/
```
4. Detailed breakdown:
```bash
gcloud storage cp ~/code/images/${IMAGE_NAME} gs://${UPLOAD_BUCKET}
```
> `gs://` is a URI scheme used for Google Cloud Storage (GCS). It indicates that the following path refers to a bucket or object stored in GCS.

- `gcloud storage cp` → Copies a file to or from a Google Cloud Storage bucket.
- `~/code/images/${IMAGE_NAME}` → Local file path (expands to something like neon.jpg).
- `gs://${UPLOAD_BUCKET}` → Destination bucket in GCS (expands to something like gs://uploaded-images-myproject).

**Common Options:**
- --recursive → Copies directories and their contents
- --preserve-posix → Preserves file permissions

### Deploy on App Engine

Run `gcloud app deploy` on the repository directory. Make sure to have a YAML configuration file within the folder.

For example, you could navigate the appropriate folder and pull the latest changes:
```bash
cd github_repos/<your_repo>
git switch main
git fetch origin
git pull
```

Then, deploy:
```bash
gcloud app deploy
```

A YAML file example:
```yaml
runtime: python39

entrypoint: gunicorn -b :$PORT app:app

handlers:
- url: /static
  static_dir: static
  http_headers:
    Cache-Control: "no-cache, no-store, must-revalidate"
    Pragma: "no-cache"
    Expires: "0"

- url: /.*
  script: auto
```

### Tail logs from a Google App Engine service
This script helps to quickly stream logs from the latest deployed App Engine version in the `default` service of the `your-project-name` project. Very useful for debugging and monitoring new deployments.

1. Set the active GCP project:
```bash
gcloud config set project your-project-name
```
Sets the current project in your `gcloud` configuration to `your-project-name`, so subsequent commands are run against this project.

2. Get the latest version ID:
```bash
LATEST_VERSION=$(gcloud app versions list \
  --service default \
  --sort-by "~version.id" \
  --limit 1 \
  --format "value(version.id)")
```

This block:
- Lists versions of the App Engine `default` service
- Sorts them in descending order by version ID (`~version.id`)
- Limits to just the most recent one (`--limit 1`)
- Extracts only the version ID (`--format "value(version.id)"`)
- Saves the result to the shell variable `LATEST_VERSION`

3. Print the latest version ID:
```bash
echo "Latest version: $LATEST_VERSION"
```
This line prints out which version was found, for clarity or debugging.

4. Tail logs from the latest version:
```bash
gcloud app logs tail \
  --service default \
  --version $LATEST_VERSION
```
Tails (continuously streams) logs for the most recent version of the `default` App Engine service.

To stop the gcloud app logs tail command from tailing logs, simply press:
```bash
Ctrl + C
````
