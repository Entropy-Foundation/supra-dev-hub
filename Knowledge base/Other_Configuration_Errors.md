# **Other_Configuration_Errors.md**

---

### ERROR SUMMARY: Incorrect Docker Mount Type

When verifying the Docker container configuration, the mount type is set to "Type": "volumes" instead of "Type": "bind".

### SOLUTION SUMMARY:

1. To ensure your Docker container has the correct mounts, run:

```PowerShell
docker inspect supra_cli
```

Check that the mount type is "Type": "bind".
If it is incorrect, please redo the last step of your setup to fix the issue.

**Link to Full Details:**
N/A
