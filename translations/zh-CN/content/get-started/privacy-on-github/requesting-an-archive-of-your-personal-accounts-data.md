---
title: 请求个人帐户数据的存档
redirect_from:
  - /articles/requesting-an-archive-of-your-personal-account-s-data
  - /articles/requesting-an-archive-of-your-personal-accounts-data
  - /github/understanding-how-github-uses-and-protects-your-data/requesting-an-archive-of-your-personal-accounts-data
intro: '{% data reusables.user-settings.export-data %}'
versions:
  fpt: '*'
  ghec: '*'
topics:
  - Policy
  - Legal
shortTitle: Request account archive
ms.openlocfilehash: f296796810978f6d884fabc699c01fbc3eabf5eb
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2022
ms.locfileid: '145128847'
---
{% data variables.product.product_name %} 存储您个人帐户活动的仓库和个人资料元数据。 您可以通过 {% data variables.product.prodname_dotcom_the_website %} 上的设置或使用用户迁移 API 导出个人帐户的数据。

有关可用于导出的数据 {% data variables.product.product_name %} 存储的详细信息，请参阅“[下载用户迁移存档](/rest/reference/migrations#download-a-user-migration-archive)”和“[关于 {% data variables.product.product_name %} 对你的数据的使用](/articles/about-github-s-use-of-your-data)”。

当你通过 {% data variables.product.prodname_dotcom_the_website %} 上的设置请求导出个人数据时，{% data variables.product.product_name %} 会将你的个人数据打包到 `tar.gz` 文件中，并向你的主电子邮件地址发送含有下载链接的电子邮件。

默认情况下，下载链接会在七天后过期。 在下载链接过期之前的任何时候，您都可以从用户设置中禁用该链接。 有关详细信息，请参阅“[删除对个人帐户数据存档的访问权限](/articles/requesting-an-archive-of-your-personal-account-s-data/#deleting-access-to-an-archive-of-your-personal-accounts-data)”。

如果你的操作系统无法本机解压缩 `tar.gz` 文件，则可以使用第三方工具来解压缩存档的文件。 有关详细信息，请参阅 Opensource.com 上的“[如何解压缩 tar.gz 文件](https://opensource.com/article/17/7/how-unzip-targz-file)”。

生成的 `tar.gz` 文件反映了在你开始数据导出时存储的数据。

## 下载个人帐户数据的存档

{% data reusables.user-settings.access_settings %} {% data reusables.user-settings.account_settings %}
3. 在“导出帐户数据”下，单击“开始导出”或“新建导出” 。
![突出显示的“开始个人数据导出”按钮](/assets/images/help/repository/export-personal-data.png)
![突出显示的“新建个人数据导出”按钮](/assets/images/help/repository/new-export.png)
4. 导出准备好供下载后，{% data variables.product.product_name %} 将发送下载链接到您的主电子邮件地址。
5. 单击电子邮件中的下载链接并在提示时重新输入密码。
6. 你将重定向到可以下载的 `tar.gz` 文件。

## 删除个人帐户数据存档的访问权限

{% data reusables.user-settings.access_settings %} {% data reusables.user-settings.account_settings %}
3. 若要在链接过期前禁用发送到你电子邮件的下载链接，请在“导出帐户数据”下，找到你想要禁用的数据导出下载，然后单击“删除”。
![突出显示的“删除个人数据导出包”按钮](/assets/images/help/repository/delete-export-personal-account-data.png)
