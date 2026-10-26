# Async URL Checker

A full-stack technical assignment, separate from my commercial client projects. It shows how I handle asynchronous state, API integration and verifiable edge cases.

[Source, setup and documented decisions](https://github.com/fsaidad/async-url-checker)

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

Follow the repository README, create two batches, switch between them, cancel one, and inspect the results. The README lists manual scenarios and quality commands. No working public deployment is claimed here.

## Boundaries

Jobs are stored in memory and lost on restart. There is no persistent queue, retry policy or distributed/global concurrency limit. HEAD-only probing may reject a site that accepts GET. A public arbitrary-URL service needs SSRF protection, authentication, quotas and abuse controls before release.

This example demonstrates implementation and explicit trade-offs; it is not evidence of years of commercial backend operation.
