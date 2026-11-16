# Issue

**Title:** Monitor resource usage

## Description

To profile SPREAD refinement, I wrote myself a relatively simple resource monitor that gathers CPU/GPU usage/memory %, logs, and then plots it. Since it gained traction in the "Xfels are great!" chatroom, I decided to generalize it and present it in a pull request. It still offers rather low time resolution, but since the last discussion, I made it more robust and extendable to different CPU/GPU architectures.

The main class to interface here can be accessed via `from libtbx.resource_monitor import ResourceMonitor` (exact file placement to be discussed). Resource monitor by default works with mpi. It collects information about individual ranks with no communication – communicating ranks offered some cool benefits, but also caused significant problems and slow-downs. The `ResourceMonitor` can be used in the following ways:

- As a decorator – to monitor every call of a given function, decorate the function definition itself. The monitor will start every time the function is called and stop every time the function is terminated:
  ```
  @ResourceMonitor(*args)
  def function_to_be_monitored()
    stuff_to_be_timed()
  ```

- As a context manager – to monitor a part of the code, place it withing
  a context manager using a `with` statement. The monitor will start at the
  start of the `with` block and terminate at the end of the `with` block:
  ```
  with ResourceMonitor(*args):
    stuff_to_be_timed()
  ```

- As a standalone instance – for more advanced or customizable application,
  the context manager can be manually instantiated, started, stopped, etc.
  The following would be roughly equivalent to context manager approach:
  ```
  rm = ResourceMonitor(*args):
  try:
    um.start()
    stuff_to_be_timed()
  finally:
    self.stop()
  ```

In particular, using the last approach I wrote a simple `cctbx.xfel.merge` `MonitorWorker` accessible via dispatch keyword `monitor`. The first (and every odd) `monitor` in `dispatch.step_list` starts a global instance of `ResourceMonitor`. The second (and every even) `monitor` stops the same global instance. Since the `monitor` worker ignors `additional_info`, I suggest appending `monitor` with meaningful suffixes, for example: `dispatch.step_list = input balance monitor_start annulus monitor_stop trumpet` to time the `annulus` worker only.

Both `ResourceMonitor` and `MonitorWorker` accept the following arguments/monitor-scope phil parameters:
- `detail: choice = *rank node rank0 none` – Detail of data to be collected: from every rank, from rank 0 only, from first rank on every node, or none.
- `period: float = 5.0` – Interval between subsequent resource statistics checks in seconds. Short periods might lead to inconsistent logging.
- `plot: bool = True` – Plot the resource usage history after the monitor is stopped.
- `prefix: str = monitor` – Filename prefix for log files and summary plot.
- `write: bool = True` – Write collected resource information to log files.

Log files are written in the working directory and feature human- and computer-readable structure that can be used to recreate plots if necessary:
```
2024-05-28 17:26:07,205 - Collecting CPU stats with self.rank_info.cpu_probe.kind=psutil
2024-05-28 17:26:07,205 - Collecting GPU stats with self.rank_info.cpu_probe.kind=Nvidia
2024-05-28 17:26:07,257 - UsageStats(cpu_usage=0.0, cpu_memory=0.28741941078671696, gpu_usage=0.0, gpu_memory=0.0418212890625)
2024-05-28 17:26:12,275 - UsageStats(cpu_usage=98.2, cpu_memory=0.2874998541016293, gpu_usage=0.0, gpu_memory=0.0418212890625)
2024-05-28 17:26:17,280 - UsageStats(cpu_usage=99.5, cpu_memory=0.2874998541016293, gpu_usage=0.0, gpu_memory=0.0418212890625)
```

Simple line plots are produced whenever `ResourceMonitor` stops i.e. whenever decorated code terminates successfully or raises an Exception:

![image](https://github.com/cctbx/cctbx_project/assets/24624030/e137da2c-58e2-4dfa-863c-496a29275f8d)
_(this image is slightly outdated, but better showcases the worker than the ones obtained in later debugging)_

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `309ccea913ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.