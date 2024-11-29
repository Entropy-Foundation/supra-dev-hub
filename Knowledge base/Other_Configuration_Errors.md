# **Other_Configuration_Errors.md**

---

## ISSUE SUMMARY: Path Issue When Binding Volumes on Windows

Docker requires an absolute path for binding volumes, but <YOUR_PATH> is not fully rooted, causing the bind to fail.
When verifying the Docker container configuration, the mount type is set to "Type": "volumes" instead of "Type": "bind".

### SOLUTION SUMMARY: For Windows, ensure <YOUR_PATH> is a fully rooted absolute path. Alternatively, guide users to navigate to the supra_configs directory on their host machine using:

1. For Windows, ensure `<YOUR_PATH>` is a fully rooted absolute path.
2. Alternatively, guide users to navigate to the `supra_configs` directory on their host machine:

```PowerShell
cd path\to\supra_configs
```

3. Execute the docker run command using ./ or ${PWD} for the path to avoid similar issues for other users.

**Link to Full Details:**
N/A
