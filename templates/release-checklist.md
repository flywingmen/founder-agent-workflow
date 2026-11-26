# Release checklist: {window name}

Owner: {callsign}  Date: {YYYY-MM-DD}

## Before the confirm
- [ ] Branches merged in order: {list of tickets and commits}
- [ ] Migration numbers unique, in order
- [ ] CI green on combined commit {sha}: run {id}, {n} passed, 0 failed, 0 skipped
- [ ] Lane click-throughs posted (1440 and 390) for each visible change
- [ ] Heavy and empty data checked on changed screens
- [ ] Blind QA run: {pass / blockers}. Any blocker caused by this window holds it.
- [ ] Founder reviewed visual changes on the test copy
- [ ] "Customers will see" list posted

## Founder's typed line
```
yes, release {window} {sha} to production
```

## Release
- [ ] Premise read from the server: live commit {sha}, build {id}, migration tip {n}
- [ ] Rollback taken and verified by content: {location}
- [ ] Migrations applied, tip before {n} after {n}
- [ ] Restarted between scheduled jobs
- [ ] Verified: build id matches, migration tip matches, {one real example of the new behaviour}
- [ ] Test copy refreshed to mirror production

## Announce
What rode: {tickets}. Released commit: {sha}. How a user does it: {clicks}.
On any failed check: roll back first, then report what failed.
