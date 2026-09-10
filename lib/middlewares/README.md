# Tari Wasmer Middlewares

The `tari-wasmer-middlewares` crate is the Tari fork of
[`wasmer-middlewares`](https://crates.io/crates/wasmer-middlewares),
published so that the Tari Ootle engine and its template test tooling can
depend on it from crates.io without a `[patch.crates-io]` entry.

It tracks the wasmer release it is built against: version `7.4.x` of this
crate is `wasmer-middlewares` `7.4.0` plus the changes below, and depends on
the stock `wasmer`, `wasmer-types` and `wasmer-vm` `7.4.0` crates.

## Changes from upstream

- `metering::MeteringGlobalIndexes` is public, as are its
  `remaining_points()` and `points_exhausted()` accessors.
- `Metering::global_indexes()` returns the indexes of the two metering
  globals once `transform_module_info` has run on a module.

Together these let a middleware pushed *after* `Metering` emit code that
reads and updates the metering globals itself. The Ootle engine uses this
to charge a length-dependent cost for bulk memory and table operators
(`memory.copy`, `memory.fill`, `memory.init`, `table.copy`, `table.fill`,
`table.grow`, `memory.grow`), whose cost the static per-operator
`Metering` cost function cannot express.

The indexes are per module and are overwritten each time the `Metering`
instance transforms one, so an instance must not be shared by engines
compiling modules concurrently. This was already true of the upstream
middleware.

The same change is proposed upstream; once it ships in a wasmer release
this fork is no longer needed.

## Contents

- `metering`: A middleware for tracking how many operators are
  executed in total and putting a limit on the total number of
  operators executed.

  [See the `metering`
  example](https://github.com/wasmerio/wasmer/blob/main/examples/metering.rs)
  to get a concrete and complete example.
