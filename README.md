# PoShFuck

PowerShell implementation of "The Fuck" (<https://github.com/nvbn/thefuck>)

When you type a command incorrectly, don't _say_ 'fuck', _type_ it!

## Installation

For PoShFuck to run, your execution policy must be lowered. So run this in an admin elevated PowerShell to install:

```powershell
Set-ExecutionPolicy RemoteSigned
iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/mattparkes/PoShFuck/master/Install-TheFucker.ps1'))
```

## Usage

We've all done this before...

```powershell
PS C:\\> peng 8.8.8.8 -a
peng : The term 'peng' is not recognized as the name of a cmdlet, function, script file, or operable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ peng 8.8.8.8 -a
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (peng:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

PoShFuck can fix it.

```powershell
PS C:\\> fuck

Did you mean?
 PING -a 8.8.8.8
[Y] Yes  [N] No  [?] Help (default is "Y"): y

Pinging google-public-dns-a.google.com [8.8.8.8] with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=15ms TTL=56
Reply from 8.8.8.8: bytes=32 time=14ms TTL=56
Reply from 8.8.8.8: bytes=32 time=14ms TTL=56
Reply from 8.8.8.8: bytes=32 time=14ms TTL=56

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 14ms, Maximum = 15ms, Average = 14ms
```

## Commands

-   `fuck` (alias to `Invoke-TheFuck`)
    This is the command which mungs your last command and presents you with options to fix it.

-   `fuck!` (alias to `Invoke-TheFuck -Force`)
    This command will execute the recommended option without prompting the user.

-   `WTF` (alias to `Get-FuckingHelp`)
    Googles your last console error.

-   `Get-Fucked`
    Prints the list of commands which you have used PoShFuck to correct previously.

## LLM Enhanced Corrections (Optional)

PoShFuck can leverage Azure OpenAI Service to provide command corrections when its built-in rules don't find a match. This significantly enhances its ability to fix a wider range of errors.

### Configuration for Azure OpenAI

To enable LLM-based corrections, you need to configure the following environment variables:

1.  **`POSHFUCK_AOAI_ENDPOINT`**: The endpoint URL of your Azure OpenAI resource (e.g., `https://your-aoai-resource.openai.azure.com/`).
2.  **`POSHFUCK_AOAI_DEPLOYMENT_NAME`**: The name of your model deployment in Azure OpenAI (e.g., `gpt-35-turbo-instruct` or your custom deployment name for a completions model).

**Authentication:**

This tool uses Azure Active Directory (AAD) authentication to connect to Azure OpenAI. It will first attempt to use Azure PowerShell, and if that fails or is unavailable, it will try to use Azure CLI.

**Primary Method: Azure PowerShell (`Az.Accounts`)**
- The `Az.Accounts` PowerShell module is preferred. If not installed, you can install it using: `Install-Module Az.Accounts -Scope CurrentUser -Force`.
- You must be logged into Azure via PowerShell. If you are not, run `Connect-AzAccount`.

**Fallback Method: Azure CLI (`az`)**
- If Azure PowerShell is not available or token acquisition fails, the script will attempt to use the Azure CLI.
- Ensure Azure CLI is installed and in your system's PATH.
- You must be logged into Azure via the CLI. If you are not, run `az login`.

**Permissions (Applicable to both methods):**
- Your AAD identity (user or service principal) needs the **"Cognitive Services OpenAI User"** role (or a role with similar data plane access permissions like `Microsoft.CognitiveServices/accounts/OpenAI/deployments/completions/action`) assigned on the Azure OpenAI resource you intend to use.

**How to set environment variables in PowerShell:**

```powershell
$env:POSHFUCK_AOAI_ENDPOINT = "YOUR_AOAI_ENDPOINT_HERE"
$env:POSHFUCK_AOAI_DEPLOYMENT_NAME = "YOUR_DEPLOYMENT_NAME_HERE"
```

To make these permanent, add them to your PowerShell profile script.

**Cost:**

Please be aware that using the Azure OpenAI service will incur costs based on your usage of the deployed models.

If these environment variables are not set, or if there's an issue communicating with the Azure OpenAI service, PoShFuck will gracefully fall back to its original pattern-matching behavior.
