Content Pipeline
===

![lfa](./lfa.png?raw=true)

# How To Read

This is a summary of the current LFA content pipeline. The explanatory
sections below are paired with the numbered arrows on the diagram
above.  As the pipeline is somewhat circular, there is no particular
"starting" location; all of the processes involved run perpetually in a
cycle. To understand the processes involved, it is probably easiest to
pick an area of the pipeline with which you are already familiar, and
follow the descriptions below at that point in order to learn how the
preceding and following stages feed into that section of the pipeline.
Once you reach the bottom of the numbered sections, start again at the
top and work your way back to where you started!

# Sections

## APK production and publication

#### P. Programmer commits code

A developer checks out the code of the various projects hosted on
[GitHub](https://www.github.com/AULFA) and commits changes.

#### O. Travis CI builds code

[Travis CI](https://www.travis-ci.org/AULFA) is configured to watch
repositories hosted in the [GitHub](https://www.github.com/AULFA) organization,
and it checks out and builds the code every time it notices that a
developer has made changes.

#### A. Travis CI uploads APK files

[Travis CI](https://www.travis-ci.org/AULFA) is configured to upload
compiled APK files to our `builds.lfa.one` server securely over
[ssh](https://en.wikipedia.org/wiki/Secure_Shell).  Developers can
also perform this step manually if they have built the code on their
own machines, and Travis CI is not available for any reason.

#### J. Repomaker reads APK files
#### I. Repomaker a generates repository file

A [repomaker](https://github.com/AULFA/repomaker) service is configured
on our `builds.lfa.one` service that watches for newly uploaded APK files and,
when new files are detected, generates a new repository file for use
by the [updater](https://github.com/AULFA/updater) application.

#### K. Opdsget fetches offline feeds
#### L. Opdsget generates offline feed archive

An [opdsget](https://github.com/AULFA/opdsget/) service is configured
on our `builds.lfa.one` server that periodically takes an offline snapshot
of content available on `cantookstation.com`. It produces an `offline.zip`
file containing the content that will be inserted into the offline reader
application.

#### M. Programmers and Travis CI download offline.zip

Programmers download the `offline.zip` content archive from our `builds.lfa.one`
server in order to build the application. Travis CI is also configured to
do this when producing builds of the application.

## Hotspot Installation / Upgrades

### For locations with usable internet connections...

#### H. Rsync synchronizes hotspots

An [rsync](https://en.wikipedia.org/wiki/Rsync) service is running that
periodically copies APK files and `updater` repository files to hotspots
that are running in locations that have usable internet connections.

### For locations with unusable internet connections...

#### N. HTTPD serves APK and repository files

A simple web server serves the currently available APK and `updater`
repository files to devices running the `updater` application.

#### D. A user downloads APK and repository files

A human can manually download APK and repository files in order to
place them onto a USB drive for updating hotspots. This step would
typically be performed in locations that _do_ have usable internet connections,
in preparation for delivering the USB drive to locations that _don't_
have usable connections.

#### E. A user delivers a USB drive or SD card

A user takes a USB drive populated with APK and repository files
to the hotspot running in any of our locations (schools).

#### F. A user inserts a USB drive into a hotspot

The hotspots are configured with a service that detects when a USB
drive has been inserted, and runs a self-update procedure that copies
the APK and repository files off the USB drive.

## Device Installation / Upgrades

#### B. Users/engineers download and install the updater

Users and engineers can download our [updater](https://github.com/AULFA/updater)
application from our `builds.lfa.one` server. The `updater` can
then fetch further updates and offer to install other applications
that we distributed.

#### C. Devices download catalogs/epubs from Cantook Station

In locations that have usable internet connections, the online version
of the app downloads EPUB files and OPDS feeds from `cantookstation.com`.

#### G. Devices download data from hotspots

In locations that are using hotspots, the `updater` application
can download APK files, repository files, EPUBs, and OPDS feeds
from hotspots.

