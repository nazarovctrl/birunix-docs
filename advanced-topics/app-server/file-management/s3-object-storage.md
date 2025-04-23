---
icon: bucket
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# S3 object storage

Biruni can store files to either oracle or S3. Database is the default. To use S3, configure the following in the .properties file:

```properties
  biruni.s3-url=
  biruni.s3-access-key=
  biruni.s3-secret-key=
  biruni.s3-bucket-name=
  biruni.s3-link-expire-time=   #S3 file load and download link expire time in hours
```

S3 file load and download link expire time is the time in hours that the file load and download links will be valid. After this time, the links will be expired and the file will not be accessible. This is a security measure to prevent unauthorized access to the files.

S3 file load and download link expire time is set to 24 hours by default. This means that the file load and download links will be valid for 24 hours after they are created. After 24 hours, the links will be expired and the file will not be accessible.

We used MinIO as the S3 object storage server. MinIO is an open source object storage server compatible with Amazon S3 APIs. MinIO is light enough to be bundled with the application. It can be downloaded from https://min.io/download.

File load and download link used in new file load and download routes. This routes returns 302 response code and redirects to the file load and download link. This is done to remove the need for the application to handle file load and download. This way, the application does not need to handle file load and download and can focus on other tasks.

### New file load and download routes are:

```properties
  load file:         b/core/m:load_file_v2
  load image:        b/core/m:load_image_v2
  download file:     b/core/m:download_file_v2
  load image public: b/core/m$load_image_v2
```
