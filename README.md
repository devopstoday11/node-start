# Node Start Cloud Native Buildpack
## `gcr.io/paketo-buildpacks/node-start`

The Paketo Node Start CNB sets the start command for a given node application.
The buildpack expects that the app contains a valid `server.js` at the root.

## Integration

This CNB writes a start command, so there's currently no scenario we can
imagine that you would need to require it as dependency. If a user likes to
include some other functionality, it can be done independent of the Node Start
CNB without requiring a dependency of it.

## Usage

To package this buildpack for consumption:

```
$ ./scripts/package.sh --version <version-number>
```

This will create a `buildpackage.cnb` file under the `build` directory which you
can use to build your app as follows:
```
pack build <app-name> -p <path-to-app> -b <path/to/node-engine.cnb> -b build/buildpackage.cnb
```

## `buildpack.yml` Configurations

There are no extra configurations for this buildpack based on `buildpack.yml`.

## Run Tests

To run all unit tests, run:
```
./scripts/unit.sh
```

To run all integration tests, run:
```
/scripts/integration.sh
```

## Graceful shutdown and signal handling

You can add signal handlers in your app to support graceful shutdown and
program interrupts. This buildpack runs the node server as the init process,
and thus it ignores any signal with the default action. As a result, the
process will not terminate on `SIGINT` or `SIGTERM` unless it is coded to do
so. You can also use docker's `--init` flag to wrap your node process with an
init system that will properly handle signals.
