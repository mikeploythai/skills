# impact

find what a change breaks elsewhere before it ships to production. use this workflow automatically when a change can escape the files being edited, especially when it affects:

* public or shared APIs, events, wire formats, configuration, or serialized data.
* database schemas, migrations, persistence, caching, or invalidation.
* authentication, authorization, permissions, secrets, or trust boundaries.
* concurrency, scheduling, lifecycle, teardown, retries, or shared mutable state.
* a dependency upgrade, deletion, broad rename, ownership move, or cross-package refactor.
* behavior consumed by multiple applications, languages, processes, or user surfaces.
* A large mechanical diff is not automatically risky. A three-line contract change may be. Skip this workflow when the change is local, reversible, and already proven by focused tests plus the project's normal verification path.

## don't trust your own judgement

return facts about a change's safety through thorough validation, not just from assumption. take each safety face as far down the ladder below as the risk warrants:

1. **located**: cite the exact contract or source that supports it.
2. **traced**: walk the failure path and show why it does or does not reach an effect.
3. **executed**: run a focused test or script against the real shipped code.
4. **reproduced**: exercise it in the running product through the relevant user or system boundary.

for material changes, aim for executable proof, such as an existing test. add a durable, focused regressions test when it protects a stable contract, otherwise create a one-off scratch reproduction that's deleted afterwards.

don't weaken assertions, test implementation details, or build a broad new harness just to make the review look complete. if proof is too expensive or unavailable, say exactly where the ladder stopped and what remains unproven.

## report

before declaring the change to be complete, report the following to me:

* **change**: the non-obvious behavioral effect.
* **safety facts**: each predicate, evidence level, and proof result.
* **confirmed risks**: only credible failure paths, with the next check or mitigation.
* **cleared**: important cases investigated and why they are safe.
* **unproven**: evidence gaps that still matter before merge.