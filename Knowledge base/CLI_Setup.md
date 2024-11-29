# **CLI_Setup.md**

---

## ISSUE SUMMARY: Supra Command Not Executing

The `supra` command does not run as expected.

```PowerShell
supra
```

### SOLUTION SUMMARY:

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

**Link to Full Details:**
N/A

## ISSUE SUMMARY: Docker Daemon Not Running

When running the following command:

```PowerShell
docker run --name supra_cli -v <YOUR_PATH>:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet-misc/supra-testnet/validator-node:v6.3.0
```

You may encounter the error:

docker: error during connect: this error may indicate that the docker daemon is not running: Post "http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/containers/create?name=supra_cli": open //./pipe/docker_engine: The system cannot find the file specified.

### SOLUTION SUMMARY:

Ensure Docker Desktop is running before executing the command
Once Docker is running, retry the command.

    ```PowerShell
    docker run --name supra_cli -v <YOUR_PATH>:/supra/configs -e SUPRA_HOME=/supra/configs --net=host -itd asia-docker.pkg.dev/supra-devnet-misc/supra-testnet/validator-node:v6.3.0
    ```

If the issue persists: - Confirm your Docker installation is complete. - Check that the Docker service is properly configured to start.

**Link to Full Details:**
N/A
