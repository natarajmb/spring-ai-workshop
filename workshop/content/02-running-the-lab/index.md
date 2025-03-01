---
title: Running the lab
---

#### Sample Application

To discover the capabilities of **Spring AI**, we will use a sample
[Spring AI Intro](https://github.com/nevenc/spring-ai-intro) repository.

The **spring-ai-intro** is a Spring Boot application that connects to an LLM.
We will use a local **Ollama** hosted model in this lab.

{{< note >}}
The lab can be used with OpenAI LLMs, but you must have your own OpenAI API key
and configure the lab in the **application.yaml** correspondingly.
{{< /note >}}

#### Trying out the Ollama Model

Let's validate that the Ollama model works in this environment by running the following request.
```execute
curl -s -X POST http://llama-proxy.{{< param workshop_namespace >}}:11434/api/generate \
     -H "Content-Type: application/json" \
     -d '{
           "model": "llama3.2",
           "prompt": "Who are you?",
           "stream": false
         }' | jq 'del(.context)'
```

{{< warning >}}
The response might be a bit slow since we are hosting the LLMs on a CPU-based system without GPUs.
{{< /warning >}}

You should see a JSON response from a **llama3.2** model hosted on a local Ollama server:

```json
{
  "model": "llama3.2",
  "created_at": "2025-02-01T10:00:00.263710817Z",
  "response": "I'm an artificial intelligence model known as Llama. Llama stands for \"Large Language Model Meta AI.\"",
  "done": true,
  "done_reason": "stop",
  "total_duration": 21750302650,
  "load_duration": 4158088904,
  "prompt_eval_count": 29,
  "prompt_eval_duration": 4185000000,
  "eval_count": 23,
  "eval_duration": 13403000000
}
```

#### Configure Application

We need to configure the application details in `application.yaml`.

```editor:open-file
file: ~/spring-ai-intro/src/main/resources/application.yaml
description: Open application.yaml configuration file
line: 3
```

Notice that we will be using `ollama` configuration.
```editor:select-matching-text
file: ~/spring-ai-intro/src/main/resources/application.yaml
description: Highlight model provider
text: "provider: ollama"
```

Notice the default Ollama base url.
```editor:select-matching-text
file: ~/spring-ai-intro/src/main/resources/application.yaml
description: Highlight the host URL for Ollama model 
text: "localhost"
start: 12
stop: 14
```

We need to substitute that with a working Ollama endpoint.
```editor:replace-text-selection
file: ~/spring-ai-intro/src/main/resources/application.yaml
description: Replace with environment-specific Ollama host URL
text: "llama-proxy.{{< param workshop_namespace >}}"
```

Notice that we are using `llama3.2` model in Ollama.
```editor:select-matching-text
file: ~/spring-ai-intro/src/main/resources/application.yaml
description: Highlight used model
text: "llama3.2"
start: 15
stop: 17
```

#### Running the Application

Let's start the application in the second terminal.
```terminal:execute
command: cd spring-ai-intro
session: 2
```

```terminal:execute
command: ./mvnw spring-boot:run
session: 2
```

#### Execute a Simple Chat Query

After the application has started successfully, we can run a simple chat query. 

Please go ahead and execute the following command in the terminal to invoke a call to the LLM.
```execute
http -b localhost:8080/chat/simple
```

Observe the output.
```
I'm an artificial intelligence model known as Llama. Llama stands for "Large Language Model Meta AI."
```

**Congratulations!** You have successfully called an LLM model from your Spring Boot application.

#### Stop the Application

You can stop the application in the second terminal.
```terminal:interrupt
session: 2
```
