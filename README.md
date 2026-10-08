# pm-mcp-parent

Parent POM `de.planmarshall:pm-mcp-parent` of every module of
[plan-marshall-mcp](https://github.com/plan-marshall/plan-marshall-mcp). It inherits from
`de.cuioss:cui-quarkus-parent` and holds what all repositories of the product share: the Java release, the licence
header, the quality-gate recipes, the managed versions of the third-party dependencies, the plugin management of
the native builds and of the coverage check, and the deployment to the package registry of the organisation
`plan-marshall`. It holds no code.

The parent is proprietary software; see [LICENSE.md](LICENSE.md). It is published to the GitHub Packages registry of
the organisation and nowhere else.

## Using it

```xml
<parent>
    <groupId>de.planmarshall</groupId>
    <artifactId>pm-mcp-parent</artifactId>
    <version>0.1.0</version>
    <relativePath />
</parent>

<properties>
    <!-- The name of the repository this POM lives in; the build fails without it -->
    <pm.repository>my-repository</pm.repository>
</properties>
```

The consuming repository commits `.mvn/settings.xml` and `.mvn/maven.config` as this repository has them, and
`src/license/header.txt` for the licence header of the `pre-commit` profile. A machine that builds needs a token for
the registry; the setup is described in the developer documentation of plan-marshall-mcp
(`doc/developer/registry-setup.adoc`).

## Releasing

Only this parent is released; the modules of the product are deployed as `SNAPSHOT` versions.

1. Make sure `main` is green and holds the POM to release. Its version stays a `SNAPSHOT`.
2. Start the workflow *Release to GitHub Packages* (`deploy.yml`) on `main` with the release version, for example
   `0.1.0`. It sets that version in the workspace, prints the effective `distributionManagement`, deploys the POM to
   `https://maven.pkg.github.com/plan-marshall/pm-mcp-parent`, and pushes the tag of the same name with the released
   POM.
3. The registry accepts a release version once. To correct a release, release the next version. If a run deployed
   but failed to push the tag, start it again with the same version: it finds the version in the registry, skips
   the deploy, and pushes the tag.
4. Raise the parent version in the consuming repositories.

The workflow receives no credential but the `GITHUB_TOKEN` of its run. It is a plain workflow of this repository until
the reusable release workflow of `cuioss/cuioss-organization` offers a GitHub Packages mode.
