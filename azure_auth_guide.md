### Azure Authentication in Python

Step 1: Save the code in PyCharm\
Step 2: Run `az login` in PowerShell to log into Azure\
Step 3: Run the script in PyCharm

**You cannot (and should not) do the login interactively inside your Python code using **``**.**

`DefaultAzureCredential()` is designed to securely pull your identity from one of the following:

1. `az login` (CLI session)
2. Managed Identity (if running inside Azure VM, Web App, etc.)
3. Environment variables
4. Visual Studio Code session
5. Others (fallbacks)

``** does not and should not prompt for username/password or open a browser — that would be insecure.**

If you want to log in from Python, you can use one of the following:

---

**Option 1: **``

```python
from azure.identity import InteractiveBrowserCredential

credential = InteractiveBrowserCredential()
```

This will open a browser window when you run the script and let you log in from there.

---

**Option 2: **``** (Not Recommended)**

```python
from azure.identity import UsernamePasswordCredential

credential = UsernamePasswordCredential(
    client_id="your-app-client-id",
    username="your-email@domain.com",
    password="your-password"
)
```

Microsoft does not recommend this method anymore.\
It is insecure and only works with certain tenants that allow username/password login (many don't).

---

### Best Practice (Local Development)

Use `DefaultAzureCredential()` with a prior `az login` — secure and automatic.\
Or use `InteractiveBrowserCredential()` for browser-based login inside your Python script.

---

**Example 1: Using **``

```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.resource import ResourceManagementClient

credential = DefaultAzureCredential()
subscription_id = "your-subscription-id"

client = ResourceManagementClient(credential, subscription_id)

for rg in client.resource_groups.list():
    print(rg.name)
```

> Run `az login` in PowerShell or terminal before executing this script.

---

**Example 2: Using **``

```python
from azure.identity import InteractiveBrowserCredential
from azure.mgmt.resource import ResourceManagementClient

credential = InteractiveBrowserCredential()
subscription_id = "your-subscription-id"

client = ResourceManagementClient(credential, subscription_id)

for rg in client.resource_groups.list():
    print(rg.name)
```

This works even if `az login` has not been run — useful for demos or systems without Azure CLI installed.

