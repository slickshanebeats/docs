---
title: Utilizar OpenID Connect con flujos de trabajo reutilizables
shortTitle: Using OpenID Connect with reusable workflows
intro: Puedes utilizar flujos de trabajo reutilizables con OIDC para estandarizar y fortalecer la seguridad de tus pasos de despliegue.
miniTocMaxHeadingLevel: 3
redirect_from:
  - /actions/deployment/security-hardening-your-deployments/using-oidc-with-your-reusable-workflows
versions:
  fpt: '*'
  ghae: issue-4757
  ghec: '*'
  ghes: '>=3.5'
type: how_to
topics:
  - Workflows
  - Security
ms.openlocfilehash: c9b5daf88f6e2dc91aad8890a3a8833cfbd2b0f0
ms.sourcegitcommit: dc42bb4a4826b414751ffa9eed38962c3e3fea8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2022
ms.locfileid: '146273054'
---
{% data reusables.actions.enterprise-beta %} {% data reusables.actions.enterprise-github-hosted-runners %}

## <a name="about-reusable-workflows"></a>Acerca de los flujos de trabajo reutilizables

En vez de copiar y pegar los jobs de despliegue de un flujo de trabajo a otro, puedes crear un flujo de trabajo reutilizable que realice los pasos de despliegue. Otro flujo de trabajo puede usar un flujo de trabajo reutilizable si cumple uno de los requisitos de acceso descritos en "[Reutilización de flujos de trabajo](/actions/learn-github-actions/reusing-workflows#access-to-reusable-workflows)".

Cuando los combinas con OpenID Connect (OIDC), los flujos de trabajo reutilizables te permiten requerir despliegues consistentes a lo largo de tus repositorios, organizaciones o empresa. Puedes hacerlo si defines las condiciones de confianza en los roles de la nube con base en los flujos reutilizables.

Para poder crear condiciones de confianza en función de flujos de trabajo reutilizables, el proveedor de servicios en la nube debe admitir las notificaciones personalizadas para `job_workflow_ref`. Esto le permite a tu proveedor de servicios en la nube identificar de qué repositorio vino originalmente el job. Si el proveedor de servicios en la nube solo admite las notificaciones estándar (_público_ y _asunto_), no podrá determinar que el trabajo se ha originado en el repositorio del flujo de trabajo reutilizable. Los proveedores de servicios en la nube que admiten `job_workflow_ref` incluyen Google Cloud Platform y HashiCorp Vault.

Antes de continuar, debe estar familiarizado con los conceptos de [flujos de trabajo reutilizables](/actions/learn-github-actions/reusing-workflows) y [OpenID Connect](/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect).

## <a name="how-the-token-works-with-reusable-workflows"></a>Cómo funciona el token con los flujos de trabajo reutilizables

Durante una ejecución de flujo de trabajo, el proveedor de OIDC de {% data variables.product.prodname_dotcom %} presenta un token de OIDC al proveedor de servicios en la nube, el cual contiene información sobre el job. Si ese trabajo forma parte de un flujo de trabajo reutilizable, el token incluirá las solicitudes estándar que contengan información sobre el flujo de trabajo que realiza la llamada y también una notificación personalizada denominada `job_workflow_ref` que contiene información sobre el flujo de trabajo que se llama.

Por ejemplo, el siguiente token de OIDC es para un job que fue parte de un flujo de trabajo llamado. Los atributos `workflow`, `ref`y otros describen el flujo de trabajo del autor de la llamada, mientras que `job_workflow_ref` hace referencia al flujo de trabajo que se llama:

```yaml{:copy}
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "example-thumbprint",
  "kid": "example-key-id"
}
{
  "jti": "example-id",
  "sub": "repo:octo-org/octo-repo:environment:prod",
  "aud": "{% ifversion ghes %}https://HOSTNAME{% else %}https://github.com{% endif %}/octo-org",
  "ref": "refs/heads/main",
  "sha": "example-sha",
  "repository": "octo-org/octo-repo",
  "repository_owner": "octo-org",
  "actor_id": "12",
  "repository_id": "74",
  "repository_owner_id": "65",
  "run_id": "example-run-id",
  "run_number": "10",
  "run_attempt": "2",
  "actor": "octocat",
  "workflow": "example-workflow",
  "head_ref": "",
  "base_ref": "",
  "event_name": "workflow_dispatch",
  "ref_type": "branch",
  "job_workflow_ref": "octo-org/octo-automation/.github/workflows/oidc.yml@refs/heads/main",
  "iss": "{% ifversion ghes %}https://HOSTNAME/_services/token{% else %}https://token.actions.githubusercontent.com{% endif %}",
  "nbf": 1632492967,
  "exp": 1632493867,
  "iat": 1632493567
}
```

Si tu flujo de trabajo reutilizable realiza pasos de despliegue, entonces será habitual que este necesite acceso a un rol específico de la nube y podría que quisieras permitir que cualquier repositorio de tu organización llamara a dicho flujo de trabajo reutilizable. Para permitir esto, deberás crear la condición de confianza que permite cualquier repositorio y flujo de trabajo llamante y luego filtrar la organización y el flujo de trabajo llamado. Consulta la siguiente sección para encontrar algunos ejemplos.

## <a name="examples"></a>Ejemplos

**Filtrado por flujos de trabajo reutilizables dentro de un repositorio específico**

Puedes configurar una solicitud personalizad que filtre por cualquier flujo de trabajo reutilizable en un repositorio específico. En este ejemplo, la ejecución del flujo de trabajo debe haberse originado en un trabajo definido en un flujo de trabajo reutilizable en el repositorio `octo-org/octo-automation` y en cualquier repositorio que sea propiedad de la organización `octo-org`.

- **Tema**:
  - Sintaxis: `repo:ORG_NAME/*`
  - Ejemplo: `repo:octo-org/*`

- **Notificación personalizada**:
  - Sintaxis: `job_workflow_ref:ORG_NAME/REPO_NAME`
  - Ejemplo: `job_workflow_ref:octo-org/octo-automation@*`

**Filtrado por un flujo de trabajo reutilizable específico en una referencia específica**

Puedes configurar una solicitud personalizada que filtre de acuerdo a los flujos de trabajo reutilizables específicos. En este ejemplo, la ejecución del flujo de trabajo debe haberse originado en un trabajo definido en el flujo de trabajo `octo-org/octo-automation/.github/workflows/deployment.yml`reutilizable y en cualquier repositorio que sea propiedad de la organización `octo-org`.

- **Tema**:
  - Sintaxis: `repo:ORG_NAME/*` 
  - Ejemplo: `repo:octo-org/*` 

- **Notificación personalizada**:
  - Sintaxis: `job_workflow_ref:ORG_NAME/REPO_NAME/.github/workflows/WORKFLOW_FILE@ref` 
  - Ejemplo: `job_workflow_ref:octo-org/octo-automation/.github/workflows/deployment.yml@ 10040c56a8c0253d69db7c1f26a0d227275512e2`
