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