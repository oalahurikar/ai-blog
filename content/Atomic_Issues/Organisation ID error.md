
GPT Assistant: 
```
Error: Error code: 401 - {'error': {'message': 'OpenAI-Organization header should match organization for API key', 'type': 'invalid_request_error', 'param': None, 'code': 'mismatched_organization'}}
```
## Solution
Added 

```.env
OPENAI_ORGANIZATION_ID=org-eVSgdkssmfkfslefbj
```

Passes api key and Org id like this

```python
org_id = get_env_value("OPENAI_ORGANIZATION_ID", "")
# Initialize OpenAI client with organization ID
self.client = OpenAI(
api_key=api_key,
organization=org_id
)
```