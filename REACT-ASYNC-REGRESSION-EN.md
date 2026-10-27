# React reliability example: keep cancellation state attached to the right job

**Own technical assignment, not a client project.** This is a controlled regression demonstration of an existing protection in Async URL Checker. The repository has one implementation commit; I am not claiming a separate historical bug-fix commit.

## User-visible problem

1. Open running job A and click **Cancel job**.
2. Before that request finishes, switch to cached running job B.
3. B must offer its own enabled **Cancel job** button. A's pending state or late error must not appear on B.

Without a component identity tied to the selected job, React can preserve the previous cancellation mutation state. B then incorrectly shows a disabled **Cancelling…** button.

## Implementation you can inspect

```tsx
<CancelJobButton key={details.id} jobId={details.id} status={details.status} />
```

The key resets the button's local mutation state when the job changes. Separately, RTK Query caches job details by job ID, and the view reads `currentData` for the active query argument. These safeguards address different kinds of stale state.

- [Job details and keyed cancellation button](https://github.com/fsaidad/async-url-checker/blob/1e1b9c3/apps/web/src/widgets/job-details/ui/JobDetails.tsx#L90)
- [Cancellation mutation and local loading/error state](https://github.com/fsaidad/async-url-checker/blob/1e1b9c3/apps/web/src/features/cancel-job/ui/CancelJobButton.tsx)
- [Existing regression test](https://github.com/fsaidad/async-url-checker/blob/1e1b9c3/apps/web/src/app/App.spec.tsx#L189)

## Verification performed on 27 October 2026

| Version | Existing targeted test | Observed outcome |
| --- | --- | --- |
| Controlled local mutation: remove only `key={details.id}` | **Fails** | Job B still exposes disabled `Cancelling…`; its own `Cancel job` button is missing |
| Original implementation restored byte for byte | **Passes** | Job B has its own enabled button; the late error for A is absent |

The test uses a deferred response and an isolated store, so it does not depend on internet speed or live servers. [Failing test output](async-controlled-regression-fails.txt) · [Passing original output](async-original-protection-passes.txt). ANSI formatting and local machine paths were removed from these logs.

Run it from the source repository after `npm ci` and `npm run build:contracts`:

```bash
npm run test -w @url-checker/web -- src/app/App.spec.tsx -t "does not carry a late cancel state"
```

To reproduce the negative control, remove that key locally, run the same command, then restore it. Do not deploy the intentionally regressed version.

The complete existing test suite also passed: **25 backend + 13 frontend tests**. The API and frontend production builds passed. No new application feature or historical client outcome is claimed by this case.

## Why it matters in paid frontend work

The same failure pattern appears in dashboards, order screens and account settings: users switch records while a request is still pending. A useful handover includes the reproduction steps, the narrow state-identity fix and a regression test that catches its removal.

[Application overview and real screenshot](ASYNC-URL-CHECKER-CASE-EN.md) · [Source repository](https://github.com/fsaidad/async-url-checker)
