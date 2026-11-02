<p align="center">
  <img width="354" src="./tfsec.png">
</p>

# tfsec-action

This repository packages tfsec as a Docker-based GitHub Action for checking Terraform configuration for security issues. It provides workflow inputs for choosing scan scope, supplying custom checks and variable values, and controlling reporting.

**Scanning is currently disabled.** The action exits successfully before downloading or running tfsec, with a comment attributing the temporary bypass to upstream rate limits. A successful workflow step therefore does not establish that Terraform was checked. Configuration inputs have no effect while this bypass remains in place.

## Using the action

You need a GitHub Actions workflow, Terraform configuration checked out into its workspace, and a Linux runner capable of running Docker actions. Add a checkout step before referencing `LydiaSolutions/tfsec-action` at a reviewed revision. Before relying on it as a security check, confirm that the selected revision actually runs the scanner and inspect the scan output; the revision documented here does not.

Earlier examples used `aquasecurity/tfsec-action`, which references the upstream action rather than this repository. Choose the repository intentionally when configuring your workflow.

## Configuration and components

The [action interface](action.yml) describes the available inputs and declares a scanner return-code output. Scan configuration includes the target directory, scanner release, output format, additional arguments, custom-check URL, and Terraform variable file. Reporting options include writing standard output to a file and requesting soft failure so findings need not stop a build. A GitHub token can authenticate release API requests to help avoid rate limiting.

The [execution logic](entrypoint.sh) contains release downloads, checksum verification, scanner installation, and the scan invocation, all currently bypassed. The [container definition](Dockerfile) supplies the shell and supporting download tools. When enabled, execution requires network access to GitHub release APIs and assets.

The existing documentation lists JSON, CSV, Checkstyle, JUnit, and SARIF output formats. Consult the [tfsec argument reference](https://aquasecurity.github.io/tfsec/v0.63.1/getting-started/usage/) for additional scan options; support depends on the selected scanner release. The declared return-code output is not populated by the current execution logic.

## Open Source Attribution

- bash: [GPL 3.0 or later](https://www.gnu.org/licenses/gpl-3.0.html)
- curl: [curl license](https://curl.se/docs/copyright.html)
- git: [GPL 2.0 or later](https://github.com/git/git/blob/master/COPYING)
- jq: [MIT](https://github.com/stedolan/jq/blob/master/COPYING)

## License

[MIT License](https://github.com/nkuik/tfsec-action/blob/master/LICENSE)
