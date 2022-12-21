---
author: hhunter-ms
ms.service: azure-dev-cli
ms.topic: include
ms.date: 12/21/2022
ms.author: hannahhunter
---

## Pre-requisites

Before you get started using `azd`, ensure you have:

- Installed:
  - [Git](https://git-scm.com/)
  - [GitHub CLI v2.3+](https://github.com/cli/cli) **(only required for `azd pipeline config` when using GitHub Actions)**
  - [Azure CLI (v 2.38.0+)](/cli/azure/install-azure-cli)
- An Azure account with an active subscription. [Create one for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## Install `azd`

### [Windows](#tab/windows)

```azdeveloper
powershell -ex AllSigned -c "Invoke-RestMethod 'https://aka.ms/install-azd.ps1' | Invoke-Expression"
```

### [Linux/MacOS](#tab/linuxmac)

```azdeveloper
curl -fsSL https://aka.ms/install-azd.sh | bash 
```

---

## Uninstall `azd`

To uninstall the `azd`:

### [Windows](#tab/windows)

```azdeveloper
powershell -ex AllSigned -c "Invoke-RestMethod 'https://aka.ms/uninstall-azd.ps1' | Invoke-Expression"
```

### [Linux/MacOS](#tab/linuxmac)

```azdeveloper
curl -fsSL https://aka.ms/uninstall-azd.sh | bash 
```

---
