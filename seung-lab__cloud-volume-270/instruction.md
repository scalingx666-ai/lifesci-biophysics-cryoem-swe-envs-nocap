# Issue

**Title:** feat: add redirection feature to info files

## Description

We were discussing a problem we are having where some of our collaborators have spreadsheets of neuroglancer links, but we are moving the layer they point to into cheaper storage. We are thinking of instrumenting Neuroglancer and CloudVolume with a redirect capability. 

I looked around for HTTP 301 support on GCS, but it didn't seem to exist, so we are proposing to handle redirects like so:

info:
```json
{
    ... may contain additional keys ... 
    "redirect": "precomputed://gs://new/location/"
}
```
The redirect process would abort with an error once a cycle is detected. 

This adds the `max_redirects` flag to CloudVolume so that you can specify how many redirects are allowed. 0 for direct access, 10 by default.

If a redirect occurs, a readonly flag is set on image upload automatically, but can be disabled.

```python
# fixing a broken redirect
vol = CloudVolume(..., max_redirects=0)
del vol.info['redirect']
vol.commit_info()

# setting images to writable 
vol = CloudVolume(...) # redirected 1 or more times
vol.image.readonly = False
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10014c3bc4a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.