## API
POS http://localhost:5001/console/api/apps/[id]/chat-messages
body
```json
{
  "response_mode": "streaming",
  "conversation_id": "",
  "files": [],
  "query": "AI有哪些应用场景",
  "inputs": {},
  "model_config": {
    "pre_prompt": "",
    "prompt_type": "simple",
    "chat_prompt_config": {},
    "completion_prompt_config": {},
    "user_input_form": [],
    "dataset_query_variable": "",
    "opening_statement": "",
    "more_like_this": {
      "enabled": false
    },
    "suggested_questions": [],
    "suggested_questions_after_answer": {
      "enabled": false
    },
    "text_to_speech": {
      "enabled": false,
      "voice": "",
      "language": ""
    },
    "speech_to_text": {
      "enabled": false
    },
    "retriever_resource": {
      "enabled": true
    },
    "sensitive_word_avoidance": {
      "enabled": false,
      "type": "",
      "configs": []
    },
    "agent_mode": {
      "enabled": false,
      "max_iteration": 5,
      "strategy": "react",
      "tools": []
    },
    "dataset_configs": {
      "retrieval_model": "multiple",
      "top_k": 4,
      "reranking_enable": false,
      "datasets": {
        "datasets": []
      }
    },
    "file_upload": {
      "image": {
        "detail": "high",
        "enabled": false,
        "number_limits": 3,
        "transfer_methods": [
          "remote_url",
          "local_file"
        ]
      },
      "enabled": false,
      "allowed_file_types": [],
      "allowed_file_extensions": [
        ".JPG",
        ".JPEG",
        ".PNG",
        ".GIF",
        ".WEBP",
        ".SVG",
        ".MP4",
        ".MOV",
        ".MPEG",
        ".MPGA"
      ],
      "allowed_file_upload_methods": [
        "remote_url",
        "local_file"
      ],
      "number_limits": 3,
      "fileUploadConfig": {
        "file_size_limit": 15,
        "batch_count_limit": 5,
        "image_file_size_limit": 10,
        "video_file_size_limit": 100,
        "audio_file_size_limit": 50,
        "workflow_file_upload_limit": 10
      }
    },
    "annotation_reply": {
      "enabled": false
    },
    "supportAnnotation": true,
    "appId": "ea3ecba3-3599-4a3c-a425-8f65c8a62f7c",
    "supportCitationHitInfo": true,
    "model": {
      "provider": "816947cf-4604-4b09-97c7-a2cb22387db3/yidingyun/dify-yidingyun-llm",
      "name": "DeepSeek-R1-BF16",
      "mode": "chat",
      "completion_params": {
        "stop": []
      }
    }
  },
  "parent_message_id": null
}
```
## Implementation Details
one conversion has many messages

