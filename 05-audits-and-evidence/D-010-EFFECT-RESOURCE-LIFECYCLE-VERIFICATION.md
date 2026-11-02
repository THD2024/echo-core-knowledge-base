# D-010 — Effect Resource Lifecycle Verification

- Date: 2026-11-02
- Status: **Verified for the tested dependency version and scenarios**
- Source repository: [THD2024/echo-core](https://github.com/THD2024/echo-core)
- Local repository HEAD used by the experiments: [`03732e8551b79da177c1fae44273dc00c4604fd9`](https://github.com/THD2024/echo-core/commit/03732e8551b79da177c1fae44273dc00c4604fd9), branch `runtime-core/lifecycle-v1`
- Dependency: `effect@4.0.0-beta.48`, confirmed from `packages/cli/node_modules/effect/package.json`
- Experiment scripts: `C:\\Temp\\d010-verify\\` (outside the repository)
- Repository integrity: `git status --porcelain` remained empty; no source, test, configuration, or dependency changes were made.
- All reported experiment commands exited with code 0.

## 1. ManagedRuntime and MemoMap behavior

### 1.1 Same MemoMap and same Layer object

Two ManagedRuntime instances were built with the same MemoMap and the same Layer object.

Observed:
- Layer construction ran once.
- Disposing Runtime 1 left the shared resource open and ran no Finalizer.
- Disposing Runtime 2 closed the resource and ran the Finalizer exactly once.

Conclusion: the shared Layer entry and its resource Scope remain alive while another observer Runtime still uses the entry. The final observer's disposal triggers the shared Scope close. A Runtime's `dispose()` must not be interpreted as proof that every resource it has observed is already released.

### 1.2 Different Layer objects providing the same Service

Two distinct Layer objects providing the same Service were built.

Observed:
- Each Layer built its own resource.
- Disposing Runtime 1 released only resource 1.
- Resource 2 remained open until Runtime 2 was disposed.
- Each Finalizer ran exactly once.

Conclusion: sharing is keyed by Layer identity/memoization key, not merely by Service type or name. Identical Service types do not imply shared resource ownership.

## 2. Finalizer order

### 2.1 Source inspection

In the tested Effect version:
- Open Scope Finalizers are stored in an insertion-ordered `Map` (`effect/src/Scope.ts:154-156`).
- Registration inserts entries through `scopeAddFinalizerUnsafe` (`effect/src/internal/effect.ts:3716-3725`).
- `scopeCloseFinalizers` converts the values to an array and iterates from the final index down to zero (`effect/src/internal/effect.ts:3655-3676`, especially `:3662-3664`).
- The default Scope strategy is `sequential` (`effect/src/internal/effect.ts:3739`).

Conclusion: on one Scope, Finalizers are invoked in reverse registration order. Under the default sequential strategy, they execute serially in that order. Under a parallel strategy, reverse iteration governs launch order, not necessarily completion order.

### 2.2 Direct Scope experiment

Registered Finalizers A, B, C on one Scope.

Observed:
```text
registered: A,B,C
finalizer C
finalizer B
finalizer A
scope state: Closed
execution order: C -> B -> A
exit: 0
```

Conclusion: direct Scope behavior confirms LIFO.

### 2.3 Layer experiments

Three resources were instrumented with acquisition and release markers.

| Composition | Observed acquisition | Observed release |
|---|---|---|
| `Layer.mergeAll(A, B, C)` | A → B → C | C → B → A |
| `provideMerge` dependency chain | A → B → C | C → B → A |
| Three `buildWithScope` calls on one explicit sequential Scope | A → B → C | C → B → A |

All three scripts exited with code 0.

Conclusion: in these tested compositions, Layer Finalizers were registered into Scope lifecycle machinery and released in reverse registration/acquisition order. `mergeAll` uses concurrent layer construction in the inspected implementation; concurrent acquisition completion order is not a general ordering guarantee. Across child Scopes, relative closure behavior also depends on parent Scope registration and strategy. Do not generalize one Scope's LIFO guarantee into a global ordering guarantee across arbitrary nested or parallel Scopes.

## 3. ManagedRuntime disposal and running Fibers

Observed:
- A Fiber started with `runtime.runFork` continued running after `ManagedRuntime.dispose()`.
- Disposal did not interrupt or wait for that business Fiber.
- The Fiber completed later while its tick count continued to advance.
- The Layer Finalizer ran during Scope close in an Effect Fiber context, rather than on the external test's synchronous caller thread.

Conclusion: ManagedRuntime disposal and business-task draining are separate responsibilities. A Fiber retaining a Service reference may continue using it after its Layer resource has been finalized. The caller/owning system must stop accepting work and drain or cancel its own tasks before releasing resources they may access.

## 4. Idempotency and concurrent disposal

Observed:
- Repeated disposal did not run the same Finalizer more than once.
- Concurrent disposal involving Runtime observers of the same shared Layer did not duplicate Finalizer execution.
- Both experiment scenarios exited with code 0.

Conclusion: the tested Effect Scope close path is idempotent for the tested scenarios. This does not remove the need for Echo-level ownership, task-drain, and failure-reporting contracts.

## 5. Architectural implications for D-010

The experiments establish the following implementation facts for `effect@4.0.0-beta.48`:

1. A shared Layer resource remains alive until the last observer of its MemoMap entry exits.
2. Layer identity matters: different Layer objects providing the same Service do not automatically share a resource.
3. Finalizers on one Scope are registered in insertion order and invoked in reverse order; sequential strategy provides serial LIFO execution.
4. ManagedRuntime disposal does not automatically interrupt or await business Fibers started through `runFork`.
5. Scope/MemoMap provide resource-scope and shared-observer lifecycle mechanics, but do not define Echo's domain-level Owner, access policy, admission stop, task draining, or process-level authority.

These findings support (but do not themselves formally approve) the following design constraints:
- Distinguish Creator, Owner, Consumer, and final Releaser; they may be the same actor for exclusive resources but must not be conflated for shared resources.
- A resource Owner is accountable for admission control, task draining, and final release.
- Feature Owners must drain/cancel their own work before releasing resources that work may access.
- Process-wide shared resources such as Database need an explicit Process Owner rather than incidental ownership by whichever Runtime first constructs them.
- Core Runtime coordinates only resources it owns or is explicitly delegated to manage; it must not take over Feature-internal resource lifecycles.
- Reuse Effect Scope, Finalizer, and MemoMap where appropriate; add Echo-specific ownership policy only where the underlying primitives do not provide it.
- Do not assume a global LIFO order across parallel or nested Scopes.
- A close operation must not initialize a resource merely to close it.

## 6. Scope and limitations

- These are local experiments and source observations for the exact dependency version `4.0.0-beta.48`; behavior may change on dependency upgrades.
- Experiments were isolated outside the repository. They did not modify source, tests, configuration, or dependencies.
- The experiments do not establish the correctness of every production Runtime factory or shutdown path.
- They do not yet decide whether Echo needs an explicit dependency DAG, resource registry, or a particular Owner API.
- D-010 architecture remains a design task; no ResourceManager implementation was started.
- This verification does not mean Core Runtime V1 or R1 is fully integrated or accepted as complete.
