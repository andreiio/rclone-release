# rclone-release

This GitHub Action takes a directory, renames, archives and uploads it via rclone.

## Usage

### Pre-requisites
Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

### Inputs

* `dest` - Upload destination, in `dest:full/path` format. Will be directly passed to rclone
* `publish_dir` - Directory to archive
* `release_name` - Name of directory that groups all the project's releases and individual release archive base name
* `release_tag` - (optional) Unique identifier that gets appended to the archive file name. Defaults to `<tag-name>_<commit-short-sha>` where tags exist and `<commit-short-sha>` for everything else.

### Outputs

* `release` - Full archive path on remote

### Example workflow

```yaml
name: Release

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v1

    - name: Release
      uses: andreiio/rclone-release@v1
      with:
        bucket: "s3:release"
        publish_dir: ./public
        release_name: project-name
      env:
        RCLONE_CONFIG_S3_TYPE: s3
        RCLONE_CONFIG_S3_ACCESS_KEY_ID: ${{ secrets.ACCESS_KEY_ID }}
        RCLONE_CONFIG_S3_SECRET_ACCESS_KEY: ${{ secrets.SECRET_ACCESS_KEY }}
```

> See the [rclone documentation on environment variables](https://rclone.org/docs/#environment-variables) for info on remote access configuration.

## License
The scripts and documentation in this project are released under the [MIT License](LICENSE)