# Puma Cloud Native Buildpack

## `gcr.io/paketo-buildpacks/puma`

The Puma CNB sets the start command for a given ruby application that runs on a [puma server](https://puma.io).

## Integration

This CNB writes a start command, so there's currently no scenario we can
imagine that you would need to require it as dependency. If a user likes to
include some other functionality, it can be done independent of the Puma CNB
without requiring a dependency of it.

## Packaging

To package this buildpack for consumption:

```bash
./scripts/package.sh --version <version-number>
```

For example:
```bash
./scripts/package.sh --version 0.4.60
```

This will build the buildpack for all target architectures specified in `buildpack.toml` (amd64 and arm64 by default) and create architecture-specific buildpackages in the `build/` directory.

## Publishing

To publish this buildpack to ECR:

```bash
# First, authenticate with ECR (if not already authenticated)
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin 348674388966.dkr.ecr.us-east-1.amazonaws.com

# Then publish the buildpack
./scripts/publish.sh \
  --image-ref 348674388966.dkr.ecr.us-east-1.amazonaws.com/neeto-deploy/paketo/buildpack/puma:<version> \
  --buildpack-type buildpack
```

For example:
```bash
./scripts/publish.sh \
  --image-ref 348674388966.dkr.ecr.us-east-1.amazonaws.com/neeto-deploy/paketo/buildpack/puma:0.4.60 \
  --buildpack-type buildpack
```

The script will automatically:
- Read target architectures from `buildpack.toml`
- Extract the buildpack archive
- Publish each architecture separately with arch-suffixed tags (e.g., `puma:0.4.60-amd64`, `puma:0.4.60-arm64`)
- Create and push a multi-arch manifest list

## `buildpack.yml` Configurations

There are no extra configurations for this buildpack based on `buildpack.yml`.
