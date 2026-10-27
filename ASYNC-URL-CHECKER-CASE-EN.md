# Async URL Checker

A full-stack technical assignment, separate from my commercial client projects. It shows how I handle asynchronous state, API integration and verifiable edge cases.

[Source, setup and documented decisions](https://github.com/fsaidad/async-url-checker)

![Actual local run: one successful URL, one HTTP 404, and one timeout](async-url-checker-preview.png)

Actual application screenshot captured on 27 October 2026 from source commit `1e1b9c3`. The URLs are public test examples. Displayed durations include the assignment's artificial processing delay; they are not a performance benchmark.

## A concrete reliability example

If a user cancels job A and switches to job B before the response returns, B must keep its own button state. [See the scenario, existing protection, controlled failing test and passing original](REACT-ASYNC-REGRESSION-EN.md). This is a reproducible demonstration of an existing implementation, separate from client work.

## The user flow

Submit a batch of URLs, follow individual HTTP-check results, switch between jobs, and cancel pending work. The interface reports loading, validation and error states.

## Engineering decisions you can inspect

- **Bounded work:** at most five worker loops per job, rather than starting one promise per URL.
- **Cancellation:** a cancelled job becomes terminal immediately. Pending items are cancelled; already issued checks may finish and retain their results.
- **Late responses:** details are cached by job ID, so a late response for a previously viewed job cannot replace the active job's screen.
- **Shared API contract:** framework-independent types connect NestJS and React/RTK Query.
- **Checks:** deterministic backend/frontend tests, HTTP API tests, type/lint checks and Docker smoke validation are documented in the repository.

[Verified main-branch CI run](https://github.com/fsaidad/async-url-checker/actions/runs/30747929299)

## Verify locally

Verified again on 27 October 2026: **25 backend tests + 13 frontend tests passed**, and both production builds passed. This verification did not rerun Docker or HTTP e2e checks; the linked CI run documents those earlier checks.

Follow the repository README, create two batches, switch between them, cancel one, and inspect the results. The README lists manual scenarios and quality commands. No working public deployment is claimed here.

## Boundaries

Jobs are stored in memory and lost on restart. There is no persistent queue, retry policy or distributed/global concurrency limit. HEAD-only probing may reject a site that accepts GET. A public arbitrary-URL service needs SSRF protection, authentication, quotas and abuse controls before release.

This example demonstrates implementation and explicit trade-offs; it is not evidence of years of commercial backend operation.
