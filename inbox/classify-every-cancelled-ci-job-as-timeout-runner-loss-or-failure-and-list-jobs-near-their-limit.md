# Classify every cancelled CI job as timeout, runner loss or failure, and list jobs near their limit

## What is wanted
Every cancelled CI job in a product's CI stream is classified by code as `timeout` (every completed
step green, total at the job's time limit), `runner-loss` (the runner received a shutdown signal —
re-run, not a failure) or `failed`, and the class is written on the job's line in the CI metrics.
A job whose slowest of its last ten runs is within a configurable margin (default 3 min) of its time
limit is listed with those durations, so the limit is raised before it flips to cancelled at random.

## Evidence (reported by the first customer install)
Two jobs hit their time limit with every step green; the landing gate refuses a cancelled job, so a
timeout blocked landings exactly like a red test. Telling the three causes apart was left to a
session remembering to read step conclusions and the runner's shutdown message. F-0043 counts
cancelled minutes and F-0070 catches a hung step; neither classifies why a job was cancelled.

## Fix direction
The CI provider interface returns step conclusions and the runner-lost marker per job; the CI
ingest derives the class; `file-bugs` keys CI signatures on it (a `runner-loss` is never a Bug; a
`timeout` near-limit trend is one S3 per job).

## Test
Three recorded provider payloads (all-green at limit, shutdown signal, failed step) produce the
three classes; a job whose last ten durations include one within the margin appears in the
near-limit list; a runner-loss files no Bug.
