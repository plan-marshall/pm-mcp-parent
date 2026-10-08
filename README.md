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
    <version>1.0.0</version>
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
