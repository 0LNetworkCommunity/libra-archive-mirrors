# TODO: upkeep of archives
Since the archives were kept by different people with different tools over time, there is some cleanup that is in progress.

As of Jul 28 2025, the archives are complete in the sense that all transactions can be found from the start of the first experimental network in Nov 2022.
However individual formats (node rocks db files, backup files) may have gaps.

## Project 1: replay the V5 one-off backup restores

There are gaps in the v5 one-off backup files as hosted on github. There are two alternative sources of state: a copy of the rocksdb directory (db-snapshot/version-five-archive), and all the transactions (exporer-data/v5-transactions).

A fullnode backup of the disk files exists for the final state of the v5 fullnode. This includes all transactions including those that required offline admin writes.

A node binary for v5 could start up with these disk files and then the backup service could run against that node and fill the gaps.

## Project 2: make a V6 archive node disk file archive

The disk files of a V6 archive node were never stored.

Solution: find v6 validators who had complete fullnodes, have them replay the state sync from genesis


## Project 3: V7 oneoff epoch archives have different file formats

The archive on github has differing formats for the archive files. Originally we started saving them without any compression type (missing a .gz). And later we did include those.
The libra backup tool will work with those cases. However attempting to use the upstream diem restore in `one off` mode will fail in some cases.
Solution: harmonize all file formats to include .gz at the end (and confirm they are in fact compressed correctly).

## Project 4: magnum opus: replay all transactions from v5 through v8 into a single clean database.
The goal is to have a single rocksdb database for all state transitions from v5 and ongoing. Ideally all consensus messages will be preserved. And all state will be readable on contemporary node tooling.

Solution: hard.
