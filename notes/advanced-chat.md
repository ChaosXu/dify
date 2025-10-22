## API
POS http://localhost:5001/console/api/apps/[id]/advanced-chat/workflows/draft/run
body
```json
{
  "files": [],
  "inputs": {
    "file": {
      "type": "document",
      "transfer_method": "local_file",
      "url": "",
      "upload_file_id": "3a907f87-c928-4f60-8325-5ecca32dd005"
    }
  },
  "query": "主要内容",
  "conversation_id": ""
}
```
## Implementation Details
```python
workflow = cls._get_workflow(app_model, invoke_from)
                return rate_limit.generate(
                    AdvancedChatAppGenerator.convert_to_event_stream(
                        AdvancedChatAppGenerator().generate(
                            app_model=app_model,
                            workflow=workflow,
                            user=user,
                            args=args,
                            invoke_from=invoke_from,
                            streaming=streaming,
                        ),
                    ),
                    request_id=request_id,
                )
```


