---
ms.topic: include
ms.date: 07/12/2022
---

Add the "AcrPull" role for the system assigned managed identity. The AcrPull role allows the App Service to pull images from the Azure Container Registry. 

In "Azure role assignments", select **+ Add role assignment** and follow the prompts to add:

* **Scope** &rarr; "Resource group"
* **Subscription** &rarr; Your subscription.
* **Resource group** &rarr; The group with the Azure Container Registry and App Service.
* **Role** &rarr; "AcrPull"

* Select **Save** to save the role.
 
For more information, see [Assign Azure roles using the Azure portal](/azure/role-based-access-control/role-assignments-portal).
