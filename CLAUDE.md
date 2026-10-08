# CLAUDE.md

Guidance for Claude Code (claude.ai/code) when working in this repository.

## Project

`pm-mcp-parent` is the parent POM of every module of plan-marshall-mcp (PM-MCP), in every repository of the
product. It holds no code. The repositories, the modules and the artifact flow are listed in the Module Structure
Specification of the product (`doc/specification/module-structure.adoc` in `plan-marshall/plan-marshall-mcp`, later
in `plan-marshall/plan-marshall-documentation`); never repeat that listing here. All project documentation lives
there, not in this repository.

## What the POM Holds

- Parent `de.cuioss:cui-quarkus-parent`, which supplies Quarkus (`version.quarkus`), cui-http and cui-java-tools.
  Never declare `version.quarkus` here.
- `dependencyManagement`: `quarkus-bom` **first** (smallrye-config convergence), then the other BOMs and the managed
  third-party versions. Never a module of the product: each repository manages its own modules.
- The Java release (`maven.compiler-plugin.release`), the proprietary licence, the `pre-commit` profile with its
  recipe list (without `UpgradeToJava21`, which would downgrade the release), the `coverage` profile, the plugin
  management of surefire, failsafe and `native-maven-plugin`, the SonarCloud organisation.
- The deployment: `distributionManagement` points at `https://maven.pkg.github.com/plan-marshall/${pm.repository}`.
  `pm.repository` has no default in the POM; this repository sets it in `.mvn/maven.config`, a consumer in its root
  POM.

## Publishing Only to the Organisation's Registry

The product is proprietary. Artifacts go to the GitHub Packages registry of the organisation `plan-marshall` and
nowhere else, never to Maven Central or another registry. Three things in the POM guard this, and none of them may be
weakened without the user's explicit decision:

- the enforcer execution `deploy-only-to-the-organisation-registry` (fails without `pm.repository`, for a foreign
  deployment target, and when `skipPublishing` is not `true`);
- the `maven-deploy-plugin` configuration that pins the target against `-DaltDeploymentRepository`;
- `central-publishing-maven-plugin` declared without its extension and with `skipPublishing`, because the cui parent
  would otherwise replace the deploy phase with a publish to Maven Central.

No workflow receives a Sonatype or GPG credential. Only this parent is released; the release is the workflow
`deploy.yml`, started by hand with the version.

## Build

- `./mvnw verify`: validates the POM and runs the enforcer rules.
- `./mvnw help:effective-pom`: shows what a consumer inherits; check `distributionManagement` after every change.
- Never add a dependency or a plugin without asking the user first.
- A change here reaches the product only through a release and the update of the parent version in each consumer.
  Before a release, install the POM locally (`./mvnw install`) and build `plan-marshall-mcp` against it.

## Git Workflow

`main` is protected by rulesets and merges go through the merge queue; direct pushes to `main` are not allowed.
Branch, commit, push, open a pull request, wait for the checks, answer and resolve every review comment. Do not
enable auto-merge and do not merge without the user's word. Commits end with
`Co-Authored-By: plan-marshall <noreply@cuioss.de>`.

CI: reusable workflows of `cuioss/cuioss-organization`, pinned by full SHA with a version comment; configuration in
`.github/project.yml`.
