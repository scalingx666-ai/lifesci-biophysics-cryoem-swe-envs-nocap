# Issue

**Title:** Copy the latest release docs to `latest`

## Description

This copies stable releases to an additional doc folder called `latest` so links at `getyank.org/latest/...` resolve correctly.

Previous attempts which only used an HTML pointer to the correct URL only worked for `...org/latest` and did not pass additional page info. S3 does not support such a feature (e.g. HTTP_REDIRECT) and making it do so would have emulated a man-in-the-middle attack, so I abandoned that path. ( for the implementation of this)

This fix has the problem in that Travis' deploy will only *add* files to the S3 bucket, and not remove the files if they don't exist. So removing/renaming pages will stick around. We would only want this for the `latest` folder so even if they  flag, we will have to be careful as to not delete old versions of the site.

I had considered using a [Travis Deploy `before_deploy`](https://docs.travis-ci.com/user/deployment/s3/#Running-commands-before-and-after-release) flag to execute an S3 command to empty out the `latest` folder on the bucket before deploy. But that will require some tests using `--dry-run` flags locally before I feel safe implementing this. @jchodera, we can talk about using the `s3cmd` in person for testing this since it involves the AWS keys which should not be shared here.

 For now though, this PR would need to happen regardless of file-cleanup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9841bc5af79b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.