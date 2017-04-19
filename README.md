# Linux Music Utils

Contains a set of scripts that configures your Linux environment suitable for Music.

Currently is capable of setting up CPU groups for a set of tools (see [Usage][usage]).

[usage]: #usage

# Usage

Create a configuration file in `~/.lmurc`:

```
# Modify to fit your cpu topology, this assumes 8 CPUs (and 4 physical cores).
# First group (known as sys) is always assigned system tasks.
# Be mindful of your physical topology since pinning to the same core tends to have better IPC
# performance.
# These can be specified either as ranges (1-4), or as groups (1,3-4).
CPUS="0-1 2-3 4-7"

# List of tools and which group to map them to <tool>:<id>
TOOLS="qjackctl:0 ardour5:1"

# USB MODEL_ID name to apply priority for
# Find out your model ID through 'nmu-usb-scan'
#USB_DEVICE="Scarlett_2i2_USB"

# IRQ mask.
# The mask defines which CPUs are dedicated towards, this is a binary mask that indicates which
# CPUs a given irq should have affinity towards.
USB_IRQ_MASK="03"
```

Now, run lmu:

Note: this will ask for sudo to setup your cpuset's, but tools _will not_ be run as root.

```bash
$> lmu
[sudo] password for udoprog (cpuset):
Setting up sys: 0-1
Setting up rt0: 2-3
Setting up rt1: 4-7
Moved 1058/1168 tasks(s) to sys cpuset
Assigning 27957 to rt0 cpuset
Assigning 27962 to rt1 cpuset
Waiting for child processes to stop...
```

At this point, `lmu` will wait until all child processes have stopped as configured in `TOOLS`.
When this is done, the cpuset will be deconfigured.

```bash
Reverting all tasks to default cpuset
... sometimes warnings
Unmounting /cpuset
```
