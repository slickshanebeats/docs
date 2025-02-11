---
title: 搜索包
intro: '您可以在 {% data variables.product.product_name %} 上搜索包，并使用搜索限定符缩小结果范围。'
product: '{% data reusables.gated-features.packages %}'
permissions: Anyone can search for packages they have access to.
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - GitHub search
redirect_from:
  - /github/searching-for-information-on-github/searching-for-packages
  - /github/searching-for-information-on-github/searching-on-github/searching-for-packages
ms.openlocfilehash: de7a348b20f18315c58ab13b2e19f0b162b9b792
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2022
ms.locfileid: '145099616'
---
{% data reusables.package_registry.packages-ghes-release-stage %}

## 关于搜索包

您可以在所有 {% data variables.product.product_name %} 中全局搜索包，也可以在特定组织内搜索包。 有关详细信息，请参阅“[关于在 {% data variables.product.prodname_dotcom %} 中搜索](/search-github/getting-started-with-searching-on-github/about-searching-on-github)”。

{% ifversion ghes %} 你只能在 {% data variables.product.product_name %} 中搜索包，而不能在 {% data variables.product.prodname_dotcom_the_website %} 上搜索，即使启用了 {% data variables.product.prodname_github_connect %} 也一样。
{% endif %}

{% data reusables.search.syntax_tips %}

## 搜索用户或组织的包

要查找特定用户或组织拥有的包，请使用 `user` 或 `org` 限定符。

| 限定符        | 示例
| ------------- | -------------
| <code>user:<em>USERNAME</em></code> | [`user:codertocat`](https://github.com/search?q=user%3Acodertocat&type=RegistryPackages) 匹配 @codertocat 所拥有的包
| <code>org:<em>ORGNAME</em></code> | [`org:github`](https://github.com/search?q=org%3Agithub&type=RegistryPackages) 匹配 {% data variables.product.prodname_dotcom %} 组织所拥有的包

## 按包可见性过滤

要按包是公共还是专用来筛选搜索，请使用 `is` 限定符。

| 限定符  | 示例 |
| ------------- | -------------
| `is:public`| [is:public angular](https://github.com/search?q=is%3Apublic+angular&type=RegistryPackages) 匹配含有文字“angular”的公共包
| `is:private`| [is:private php](https://github.com/search?q=is%3Aprivate+php&type=RegistryPackages) 匹配含有文字“php”的专用包
