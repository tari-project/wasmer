# Wasmer Middlewares

The `tari-wasmer-middlewares` crate (Tari fork of `wasmer-middlewares`) is a collection of various useful
middlewares:

- `metering`: A middleware for tracking how many operators are
  executed in total and putting a limit on the total number of
  operators executed.

  [See the `metering`
  example](https://github.com/wasmerio/wasmer/blob/main/examples/metering.rs)
  to get a concrete and complete example.
