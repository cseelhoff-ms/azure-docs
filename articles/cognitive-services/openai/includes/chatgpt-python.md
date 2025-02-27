---
title: 'Quickstart: Use Azure OpenAI Service with the Python SDK'
titleSuffix: Azure OpenAI
description: Walkthrough on how to get started with Azure OpenAI and make your first completions call with the Python SDK. 
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: openai
ms.topic: include
author: mrbullwinkle
ms.author: mbullwin
ms.date: 03/21/2023
keywords: 
---

[Library source code](https://github.com/openai/openai-python?azure-portal=true) | [Package (PyPi)](https://pypi.org/project/openai?azure-portal=true) |

## Prerequisites

- An Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services?azure-portal=true)
- Access granted to Azure OpenAI Service in the desired Azure subscription.
    Currently, access to this service is granted only by application. You can apply for access to Azure OpenAI Service by completing the form at [https://aka.ms/oai/access](https://aka.ms/oai/access?azure-portal=true).
- [Python 3.7.1 or later version](https://www.python.org?azure-portal=true).
- The following Python libraries: os.
- An Azure OpenAI Service resource with either the `gpt-35-turbo` or the `gpt-4`<sup>1</sup> models deployed. For more information about model deployment, see the [resource deployment guide](../how-to/create-resource.md).

<sup>1</sup> **GPT-4 models are currently only available by request.** Existing Azure OpenAI customers can [apply for access by filling out this form](https://aka.ms/oai/get-gpt4).

> [!div class="nextstepaction"]
> [I ran into an issue with the prerequisites.](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=AOAI&Product=Chatgpt&Page=quickstart&Section=Prerequisites)

## Set up

Install the OpenAI Python client library with:

```console
pip install openai
```

> [!NOTE]
> This library is maintained by OpenAI and is currently in preview. Refer to the [release history](https://github.com/openai/openai-python/releases) or the [version.py commit history](https://github.com/openai/openai-python/commits/main/openai/version.py) to track the latest updates to the library.

### Retrieve key and endpoint

To successfully make a call against Azure OpenAI, you'll need an **endpoint** and a **key**.

|Variable name | Value |
|--------------------------|-------------|
| `ENDPOINT`               | This value can be found in the **Keys & Endpoint** section when examining your resource from the Azure portal. Alternatively, you can find the value in the **Azure OpenAI Studio** > **Playground** > **Code View**. An example endpoint is: `https://docs-test-001.openai.azure.com/`.|
| `API-KEY` | This value can be found in the **Keys & Endpoint** section when examining your resource from the Azure portal. You can use either `KEY1` or `KEY2`.|

Go to your resource in the Azure portal. The **Endpoint and Keys** can be found in the **Resource Management** section. Copy your endpoint and access key as you'll need both for authenticating your API calls. You can use either `KEY1` or `KEY2`. Always having two keys allows you to securely rotate and regenerate keys without causing a service disruption.

:::image type="content" source="../media/quickstarts/endpoint.png" alt-text="Screenshot of the overview UI for an OpenAI Resource in the Azure portal with the endpoint & access keys location circled in red." lightbox="../media/quickstarts/endpoint.png":::

Create and assign persistent environment variables for your key and endpoint.

### Environment variables

# [Command Line](#tab/command-line)

```CMD
setx AZURE_OPENAI_KEY "REPLACE_WITH_YOUR_KEY_VALUE_HERE" 
```

```CMD
setx AZURE_OPENAI_ENDPOINT "REPLACE_WITH_YOUR_ENDPOINT_HERE" 
```

# [PowerShell](#tab/powershell)

```powershell
[System.Environment]::SetEnvironmentVariable('AZURE_OPENAI_KEY', 'REPLACE_WITH_YOUR_KEY_VALUE_HERE', 'User')
```

```powershell
[System.Environment]::SetEnvironmentVariable('AZURE_OPENAI_ENDPOINT', 'REPLACE_WITH_YOUR_ENDPOINT_HERE', 'User')
```

# [Bash](#tab/bash)

```Bash
echo export AZURE_OPENAI_KEY="REPLACE_WITH_YOUR_KEY_VALUE_HERE" >> /etc/environment && source /etc/environment
```

```Bash
echo export AZURE_OPENAI_ENDPOINT="REPLACE_WITH_YOUR_ENDPOINT_HERE" >> /etc/environment && source /etc/environment
```
---

> [!div class="nextstepaction"]
> [I ran into an issue with the setup.](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=AOAI&Product=Chatgpt&Page=quickstart&Section=Set-up)

## Create a new Python application

1. Create a new Python file called quickstart.py. Then open it up in your preferred editor or IDE.

2. Replace the contents of quickstart.py with the following code. You need to set the `engine` variable to the deployment name you chose when you deployed the GPT-35-Turbo or GPT-4 models. Entering the model name will result in an error unless you chose a deployment name that is identical to the underlying model name.

    ```python
    #Note: The openai-python library support for Azure OpenAI is in preview.
    import os
    import openai
    openai.api_type = "azure"
    openai.api_base = os.getenv("AZURE_OPENAI_ENDPOINT") 
    openai.api_version = "2023-05-15"
    openai.api_key = os.getenv("AZURE_OPENAI_KEY")

    response = openai.ChatCompletion.create(
        engine="gpt-35-turbo", # engine = "deployment_name".
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": "Does Azure OpenAI support customer managed keys?"},
            {"role": "assistant", "content": "Yes, customer managed keys are supported by Azure OpenAI."},
            {"role": "user", "content": "Do other Azure Cognitive Services support this too?"}
        ]
    )

    print(response)
    print(response['choices'][0]['message']['content'])
    ```

    > [!IMPORTANT]
    > For production, use a secure way of storing and accessing your credentials like [Azure Key Vault](../../../key-vault/general/overview.md). For more information about credential security, see the Cognitive Services [security](../../security-features.md) article.

3. Run the application with the `python` command on your quickstart file:

    ```console
    python quickstart.py
    ```

## Output

```console
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "Yes, most of the Azure Cognitive Services support customer managed keys. However, not all services support it. You can check the documentation of each service to confirm if customer managed keys are supported.",
        "role": "assistant"
      }
    }
  ],
  "created": 1679001781,
  "id": "chatcmpl-6upLpNYYOx2AhoOYxl9UgJvF4aPpR",
  "model": "gpt-3.5-turbo-0301",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 39,
    "prompt_tokens": 58,
    "total_tokens": 97
  }
}
Yes, most of the Azure Cognitive Services support customer managed keys. However, not all services support it. You can check the documentation of each service to confirm if customer managed keys are supported.
```

> [!div class="nextstepaction"]
> [I ran into an issue when running the code sample.](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=AOAI&Product=Chatgpt&Page=quickstart&Section=Create-python-application)

### Understanding the message structure

The GPT-35-Turbo and GPT-4 models are optimized to work with inputs formatted as a conversation.  The `messages` variable passes an array of dictionaries with different roles in the conversation delineated by system, user, and assistant. The system message can be used to prime the model by including context or instructions on how the model should respond.

The [GPT-35-Turbo & GPT-4 how-to guide](../how-to/chatgpt.md) provides an in-depth introduction into the options for communicating with these new models.

## Clean up resources

If you want to clean up and remove an OpenAI resource, you can delete the resource. Before deleting the resource, you must first delete any deployed models.

- [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
- [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## Next steps

* Learn more about how to work with GPT-35-Turbo and the GPT-4 models with [our how-to guide](../how-to/chatgpt.md).
* For more examples, check out the [Azure OpenAI Samples GitHub repository](https://aka.ms/AOAICodeSamples)
