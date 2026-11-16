# Issue

**Title:** Feat: retry command requests when URLError is raised

## Description

URLError is raised when the command client can't connect to the command server. This may happen during server restarts or temporary losses in connection. With this change, if the connection is restored within 1 minute (by default), the request succeeds. Otherwise, it raises an error like before.

Example output (with shortened retry interval):

```py
❯ CRYOSPARC_COMMAND_RETRY_SECONDS=5 python
Python 3.8.17 | packaged by conda-forge | (default, Jun 16 2023, 07:06:00) 
[GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from cryosparc.command import CommandClient
>>> cli = CommandClient(port=61102)
>>> # command server goes down
>>> cli.show_scheduler_targets()
<stdin>:1: UserWarning: *** CommandClient: (http://localhost:61102/api) URL Error [Errno 111] Connection refused, attempt 1 of 3. Retrying in 5 seconds
<stdin>:1: UserWarning: *** CommandClient: (http://localhost:61102/api) URL Error [Errno 111] Connection refused, attempt 2 of 3. Retrying in 5 seconds
Traceback (most recent call last):
  File "/u/nfrasser/ecl/cryosparc_package/cryosparc_master/cryosparc_tools/cryosparc/command.py", line 110, in func
    with make_json_request(self, "/api", data=data, _stacklevel=4) as request:
  File "/u/nfrasser/ecl/cryosparc_package/cryosparc_master/deps/anaconda/envs/cryosparc_master_env/lib/python3.8/contextlib.py", line 113, in __enter__
    return next(self.gen)
  File "/u/nfrasser/ecl/cryosparc_package/cryosparc_master/cryosparc_tools/cryosparc/command.py", line 210, in make_request
    raise CommandClient.Error(client, error_reason, url=url)
cryosparc.command.Error: *** CommandClient: (http://localhost:61102/api) URL Error [Errno 111] Connection refused

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/u/nfrasser/ecl/cryosparc_package/cryosparc_master/cryosparc_tools/cryosparc/command.py", line 113, in func
    raise CommandClient.Error(
cryosparc.command.Error: *** CommandClient: (http://localhost:61102) Did not receive a JSON response from method "show_scheduler_targets" with params ()
>>>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47ac1c017c3b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.