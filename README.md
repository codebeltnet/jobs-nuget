# Reusable Workflows for NuGet by Microsoft

This repository contains reusable workflows for deploying NuGet packages from within your CI/CD pipeline.

> These workflows is part of the Codebelt umbrella and ensures a consistent way of: 
> 
> - Defining your CI/CD pipeline 
> - Structuring your repository
> - Keeping your codebase small and feasible
> - Writing clean and maintainable code
> - Deploying your code to different environments
> - Automating as much as possible
>
> A paved path to excel as a DevSecOps Engineer.

## Available Workflows

- [default.yml](.github/workflows/default.yml) - the default workflow that:
  - determines the job to invoke based on the environment input,
  - add the Mono repository,
  - installs the Mono package,
  - [deployes the NuGet package](https://github.com/codebeltnet/nuget-push).

### Usage

To call this workflow in your GitHub repository, you can follow these steps:

```yaml
nuget-call:
    uses: codebeltnet/jobs-nuget/.github/workflows/default.yml@v1
```

### Inputs

```yaml
with:
  # The version of your project, e.g., 1.0.0.
  version:
  # When specified, the environment secret will be used, and not the secret passed from the caller workflow.
  environment:
  # Defines the build configuration. Default is Release.
  configuration:
  # Sets the verbosity level of the command. Allowed values are quiet, normal and detailed. The default is quiet.
  level:
  # The name of the downloaded build artifact. Default, when left empty, is 'format('NuGet-{0}', inputs.configuration)'.
  downloadBuildArtifactName:
  # The maximum time in minutes to allow the job to run. Default is 15 minutes.
  timeout-minutes: 15
```

### Secrets

```yaml
secrets:
  NUGET_TOKEN: ${{ secrets.NUGET_TOKEN }}
```

### Outputs

This workflow has no outputs.

### Example

```yaml
jobs:
  deploy:
    if: github.event_name != 'pull_request'
    name: call-nuget
    needs: [build,pack,test,sonarcloud,codecov,codeql]
    uses: codebeltnet/jobs-nuget/.github/workflows/default.yml@v1
    with:
      version: ${{ needs.build.outputs.version }}
      environment: Production
      configuration: ${{ inputs.configuration == '' && 'Release' || inputs.configuration }}
    secrets: inherit
```

## Contributing to Reusable Workflows for NuGet by Microsoft

Contributions are welcome! 
Feel free to submit issues, feature requests, or pull requests to help improve these workflows.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
