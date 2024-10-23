---
tags:
  - openai-api
---
>[!danger] change these images later

![[Pasted image 20241018072307.png]]


![[Pasted image 20241018072443.png]]


General comparison

| Aspect                   | Assistants API                                                                                                                                                                      | Chat Completions API                                                                                                 |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Initial Setup**        | Create an Assistant with defined capabilities.                                                                                                                                      | No explicit setup of an Assistant is required.                                                                       |
| **Session Management**   | Initiate and manage a thread for ongoing conversations.                                                                                                                             | No explicit session or thread management; each request is independent.                                               |
| **Interaction Handling** | Interact through the Runs API, considering the entire conversation context.                                                                                                         | Send the entire chat history in each request, including system prompts and previous interactions.                    |
| **Context Management**   | Persistent context through the thread, suitable for extended conversations.                                                                                                         | Context is provided in each request; best for single interactions or where full context is included each time.       |
| **Complexity**           | More complex setup, offering detailed control and customization.                                                                                                                    | Simpler and more straightforward, with less granular control.                                                        |
| **Ideal Use Cases**      | Best for detailed, context-heavy conversational applications.                                                                                                                       | Suited for simpler chatbots or applications where each response is standalone.                                       |
| **Capabilities**         | Advanced capabilities like integration with a code interpreter, online search for information queries, the ability to retrieve knowledge from uploaded files, and function calling. | Primarily focused on function calling, with less emphasis on extended capabilities beyond generating text responses. |

