Skt data flow and reporting design
==================================

Each skt stage command has:

* Two inputs:
    * Command-line options
        * One of which may be specifying a configuration file containing
          some command-line option values. The file cannot be used to
          specify anything, which cannot be set with command-line options.
          Command-line options following this option override values in
          the file. This option can have a default, such as ~/.skt.yaml
    * Input directory
* Two outputs:
    * Output directory
    * Exit status summarizing the produced contents of the output
      directory. I.e. the output directory contains all the data encoded
      in the exit status.

Among skt options are: input directory and output directory
input directory by default is current directory
output directory by default is input directory

None of the different skt stage commands have output file names named the
same, so they cannot step onto each other's toes. However, re-running the same
skt stage command replaces existing files completely. E.g. it will need to
remove all files that might have been created by the previous run of the same
command, before starting adding its own, so the output file set is not
inconsistent.

Output of one skt stage command is a superset of an input to the next stage
command. Note: input and output don't have to be in the same directory. I.e.
output of one stage can be input to multiple runs of the next stage, e.g. with
different command-line options.

A stage command shouldn't require repeating the option passed to any of the
previous stages (i.e. those should be encoded in the stage's output), but may
allow overriding them.

In essence, an output of a stage is an independent entity, using it should not
require providing any other input to skt.

Each command has inputs and outputs documented explicitly, so they could be
used both by skt and other programs, however some parts might be defined
opaque for use by skt only. Specific command descriptions follow.

All stages
----------

### Report text
Each stage output includes a file containing a human-readable report text,
summarizing what happened during the stage. That text can then be included
into the overall report by sktm. The text can refer to other files output by
the stage, using special formatting, which can then be replaced by skt by
e.g. references to e-mail attachments or hyperlinks.

The syntax of such references is `{<NAME>}`, where `<NAME>` is the name of the
output file to include into the report. To provide an option to include
literal curly braces into reports, any characters preceded by a backslash
would be copied by sktm into the summarized report literally, without
interpretation.

Merge
-----

Produce a source tree with patches applied.

### Inputs

#### Command-line options

* git repository to fetch from
* git reference from the repository to base patches on
* git fetch depth
* what to merge on top (in order of specification),
  arbitrary combination and number:
    * git reference from the same repository
    * URL of a patch mbox (including file:// scheme)

#### Input directory

No files expected

### Outputs

#### Output directory

* "source" directory containing what is required as the input by the
  "Build" command.
* "merge.log" - diagnostics output of the merging attempt.
* "merge.report" - report text, as described above
* "merge.done" - empty file, created only if merge stage succeeded

#### Exit status
Zero on success, one if "merge.done" is missing, two and greater if other
failure occurred.

Build
-----

Produce a kernel tarball.

### Inputs

#### Command-line options

* Extra make options
* Configuration type: "tinyconfig", "rh-configs" (works only if Makefile
  supports "rh-configs" target), other configuration-generating target
  supported by the Makefile in "source".
* Shell glob, relative to the "source" root, matching a configuration file
  inside the "source" tree to use. Only valid if "rh-configs"
  configuration type is used.
* Kernel configuration file to use as the base. Exclusive with
  "tinyconfig" and "rh-configs" configuration types.

TODO: Figure out a better arrangement, so values, not options are exclusive.
TODO: See if we can define allowed configuration types more strictly.

#### Input directory

* "source" directory containing the source of the "kernel", must contain
  the following (TODO: make sure everything we need is listed):
    * Makefile
        * Must support the following targets (TODO: detail all targets):
            * "mrproper" - cleanup the tree
            * "tinyconfig" - generate minimum kernel configuration
            * "kernelrelease" - output kernel version
            * "targz-pkg" - build kernel tarball
        * May support the following targets:
            * "rh-configs" - generate a set of configuration files
              somewhere in the build directory
            * other targets generating kernel configuration
* "merge.done" - empty file

### Outputs

#### Output directory

* "kernel.tar.gz" - the built kernel tarball, as required by "run" input.
* "kernel.config" - the used kernel configuration
* "build.log" - diagnostics output of the building stage
* "build.report" - report text, as described above
* "build.done" - empty file, created only if build stage succeeded

#### Exit status
Zero on success, one if "build.done" is missing, two and greater if other
failure occurred.

Run
---

Run tests on a kernel.

### Inputs

#### Command-line options

* Runner type
    * "beaker" - run tests in Beaker. Requires Beaker client ("bkr")
      configured for access to a Beaker service.
* Runner parameters
    * "beaker"
        * Beaker job XML template
        * Beaker user to run the job as. Optional.

#### Input directory

* "kernel.tar.gz" - the built kernel tarball

  TODO: Describe minimal requirements to the tarball, so that we can
  substitute this with a minimal "kernel" for testing.

#### Output directory

* "run.report" - report text, as described above
* "run.done" - empty file, created only if run stage completed.

TODO: Describe representation of test results