## Table of Contents
1. [> Supra Command Not Executing](#ISSUE-SUMMARY-Supra-Command-Not-Executing)
2. [> Docker Daemon Not Running](#ISSUE-SUMMARY-Docker-Daemon-Not-Running)
3. [> Unable to find image 'asia-docker.pkg.dev/supra-devnet/misc/supra-testnet/validator-node:v6.3.0' locally](#ISSUE-SUMMARY-Unable-to-find-image)
4. [> Supra equivalent command for create-resource-account-and-publish-package](#ISSUE-SUMMARY-Supra-equivalent-command-for)
5. [> Not able to locate MOVE.Toml file](#issue-summary-trouble-for-seeing-movetoml-file-at-local-but-only-on-docker-container-files)

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

NOTE: Follow the format below to get started with reporting the issues!
- `ISSUE SUMMARY`
- `SOLUTION SUMMARY`

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Supra Command Not Executing

The `supra` command does not run as expected.

```PowerShell
supra
```

### ➥ SOLUTION SUMMARY:

1. Verify that aliasing is properly set for your Docker containers.
2. Check if your Docker container is running in Docker Desktop:

   - Open Docker Desktop and ensure the container is active, or
   - Run the following command:

```PowerShell
docker ps
```

   Ensure the supra_cli container is listed as running.

3. If the container is not running:
   - Restart the container.
   - Attempt to run the `supra` command again.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Docker Daemon Not Running

When running the following command:

```PowerShell
docker run --name supra_cli -v <YOUR_PATH>:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet-misc/supra-testnet/validator-node:v6.3.0
```

You may encounter the error:

`
docker: error during connect: this error may indicate that the docker daemon is not running: Post "http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/containers/create?name=supra_cli": open //./pipe/docker_engine: The system cannot find the file specified.
`

### ➥ SOLUTION SUMMARY:

Ensure Docker Desktop is running before executing the command
Once Docker is running, retry the command.

```PowerShell
docker run --name supra_cli -v <YOUR_PATH>:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet-misc/supra-testnet/validator-node:v6.3.0
```

If the issue persists: - Confirm your Docker installation is complete. - Check that the Docker service is properly configured to start.

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Unable to find image:

/ Terminal Command
```PowerShell
docker run --name supra_cli -v <YOUR_PATH>:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet- misc/supra-testnet/validator-node:v6.3.0
```
/ Response
```PowerShell
Unable to find image 'asia-docker.pkg.dev/supra-devnet/misc/supra-testnet/validator-node:v6.3.0' locally
docker: Error response from daemon: Head "https://asia-docker.pkg.dev/v2/supra-devnet/misc/supra-testnet/validator-node/manifests/v6.3.0": denied: Unauthenticated request. Unauthenticated requests do not have permission "artifactregistry.repositories.downloadArtifacts" on resource "projects/supra-devnet/locations/asia/repositories/misc" (or it may not exist).
```
### ➥ SOLUTION SUMMARY:

Validator Version Wrong, Check validator version, Right Command:
   ```PowerShell
docker run --name supra_cli -v /Users/danielwarren/Documents/code/hackathons/keystone-labs/permissionless-iii/apps/contracts/supra/supra_configs:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet- misc/supra-testnet/validator-node:v6.3.0
   ```

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: Supra equivalent command for

```PowerShell
aptos move create-resource-account-and-publish-package
```
### ➥ SOLUTION SUMMARY:
We don't have a CLI command for this directly, you call the create_resource_account function manually, here is the GitHub ref to check its execution: https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/supra-framework/sources/resource_account.move

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## ISSUE SUMMARY: trouble for seeing MOVE.Toml file at local but only on Docker Container files

### ➥ SOLUTION SUMMARY:
Use the below command and give the path to your main folder where the project is and then check in the docker container if it's reflecting:

```PowerShell
docker run --name supra_cli -v ${PWD}:/supra/configs/<PATH LINK> -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet-misc/supra-testnet/validator-node:v6.3.0
```

Also for Move.toml issue, the one in the local project folder has to be the same as the Move.toml file in your Docker container.

Click on supra container > files > Supra folder > config > Move workspace > check the move.toml file via code editor and make sure it has the same address and dependencies.
