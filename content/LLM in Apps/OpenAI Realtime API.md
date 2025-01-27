---
tags:
  - openai-api
draft: true
backlinks: "[[Web socket]]"
---
# Understanding the OpenAI Realtime API Integration

### WebSocket Connection:
• The script connects to the OpenAI Realtime API using a WebSocket, which allows for real-time, bidirectional communication.
• Authentication is handled via headers, including the Authorization and OpenAI-Beta headers.

Option 1:
```python
#main.py
# Suggested restructuring:

class WebSocketManager:
    """Handles WebSocket connection and basic message handling"""
    # Move WebSocket-specific methods here
    
class EventHandler:
    """Handles different types of events from the WebSocket"""
    # Move event handling logic here
    
class AudioManager:
    """Manages audio recording and playback"""
    # Move audio-related functionality here
    
class FunctionCallHandler:
    """Handles function calls and their responses"""
    # Move function call logic here

class RealtimeAPI:
    def __init__(self, prompts=None):
        self.ws_manager = WebSocketManager()
        self.event_handler = EventHandler()
        self.audio_manager = AudioManager()
        self.function_handler = FunctionCallHandler()
        # ... rest of initialization
```

Option 2:

```python
class WebSocketClient:
    def __init__(self, websocket):
        self.websocket = websocket

    async def send(self, event_type: str, content: Dict):
        message = {"type": event_type, **content}
        log_ws_event("Outgoing", message)
        await self.websocket.send(json.dumps(message))

    async def receive(self) -> Dict:
        message = await self.websocket.recv()
        event = json.loads(message)
        log_ws_event("Incoming", event)
        return event
```

### Session Configuration:
A session.update message is sent to configure the session settings:
• **Modalities:** Specifies the types of communication (e.g., text, audio).
• **Voice Settings:** Configures the assistant’s voice for audio responses.
• **Turn Detection:** Sets parameters for detecting when the user has finished speaking.
• **Tools:** Defines functions that the assistant can call to perform specific tasks.

>[!info] Use a configuration file (e.g., YAML or JSON) or a configuration class.

Option 1
```python
# config.py
class Config:
    API_URL = "wss://api.openai.com/v1/realtime"
    MODEL = "gpt-4o-realtime-preview-2024-10-01"
    HEADERS = {
        "Authorization": f"Bearer {os.getenv('OPENAI_API_KEY')}",
        "OpenAI-Beta": "realtime=v1",
    }
```

Option 2
```python
# config.py
from dataclasses import dataclass

@dataclass
class WSConfig:
    url: str = "wss://api.openai.com/v1/realtime?model=gpt-4o-realtime-preview-2024-10-01"
    ping_interval: int = 30
    ping_timeout: int = 10
    close_timeout: int = 120

@dataclass
class AudioConfig:
    silence_threshold: float = SILENCE_THRESHOLD
    prefix_padding_ms: int = PREFIX_PADDING_MS
    silence_duration_ms: int = SILENCE_DURATION_MS
```

### Audio Event Handling:
• The assistant and the user exchange messages through events with specific types, such as response.text.delta for text responses or response.audio.delta for audio responses.
• Function calls are handled through events like ~~response.function_call_arguments.delta~~ and ~~response.function_call_arguments.done~~.
Use efficient data structures or libraries specialized in audio processing.

Option1:
```python
async def handle_event(self, event, websocket):
    event_type = event.get("type").replace('.', '_')
    handler = getattr(self, f"handle_{event_type}", None)
    if handler:
        await handler(event, websocket)
    else:
        logger.warning(f"Unhandled event type: {event_type}")

async def handle_response_created(self, event, websocket):
    # Handle response.created event
    ...

async def handle_response_text_delta(self, event, websocket):
    # Handle response.text.delta event
    ...
```

Option2:
```python
class EventHandler:
    def __init__(self, api_instance):
        self.api_instance = api_instance
        self.event_map = {
            "response.created": self.handle_response_created,
            "response.text.delta": self.handle_response_text_delta,
            # Add other event handlers here
        }

    async def handle(self, event, websocket):
        event_type = event.get("type")
        handler = self.event_map.get(event_type)
        if handler:
            await handler(event, websocket)
        else:
            logger.warning(f"Unhandled event type: {event_type}")

    async def handle_response_created(self, event, websocket):
        self.api_instance.mic.start_receiving()
        self.api_instance.state.response_in_progress = True

    async def handle_response_text_delta(self, event, websocket):
        delta = event.get("delta", "")
        self.api_instance.state.assistant_reply += delta
        print(f"Assistant: {delta}", end="", flush=True)
```

### Function Calls:
• The assistant can request to execute functions defined in the tools module.
• The script executes the requested function and sends the output back to the assistant to be included in the conversation.

**Error Handling**:
Implement a consistent error-handling strategy using custom exceptions and centralized error logging.

```python
# error_handling.py

class RealtimeAPIError(Exception):
    pass

async def handle_error(self, event, websocket):
    error_details = event.get("error", {})
    error_message = error_details.get("message", "Unknown error")
    raise RealtimeAPIError(error_message)
    
class WebSocketError(Exception):
    """Base class for WebSocket-related errors"""
    pass

class AudioError(Exception):
    """Base class for audio-related errors"""
    pass

class APIError(Exception):
    """Base class for API-related errors"""
    pass
```

### State Management:

```python
#state.py
from enum import Enum
from dataclasses import dataclass

class ConnectionState(Enum):
    DISCONNECTED = "disconnected"
    CONNECTING = "connecting"
    CONNECTED = "connected"
    ERROR = "error"

@dataclass
class APIState:
    connection_state: ConnectionState
    is_processing: bool
    current_function_call: dict | None
    audio_buffer: list
```

