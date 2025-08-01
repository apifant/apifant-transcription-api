# Apifant Transcription API

## Overview

The **Apifant Transcription API** provides advanced audio transcription services with multiple specialized transcription methods. Transform your audio content into accurate, timestamped text with speaker identification and multi-language support.

### Key Features

- **Multiple Transcription Modes**: Full-text, quick mode, and speaker diarization
- **Multi-Language Support**: 20+ languages with automatic detection
- **Speaker Identification**: Advanced diarization for multi-speaker audio
- **Real-time & Batch Processing**: Synchronous and asynchronous processing options
- **Webhook Integration**: Real-time notifications for batch processing
- **Detailed Metrics**: Performance insights and audio analysis
- **Multiple Audio Formats**: MP3, WAV support (up to 300MB)

## Quick Start

### 1. Basic Usage

```bash
curl -X POST \
  'https://your-endpoint.apifant.com/api/transcribe/speech?mode=fulltext&source_lang=de&target_lang=de' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'Content-Type: audio/wav' \
  --data-binary '@your-audio-file.wav'
```

### 2. Response Example

```json
{
  "full_text": "Hello, how are you today? I'm doing great, thank you for asking.",
  "words": [
    {
      "timestamp": { "start": 0.1, "end": 0.5 },
      "text": "Hello",
      "speaker": "SPEAKER_00"
    },
    ...
  ],
  "sentences": [
    {
      "timestamp": { "start": 0.1, "end": 2.3 },
      "text": "Hello, how are you today?",
      "speaker": "SPEAKER_00"
    }
  ],
  "metrics": {
    "transcription_time": 1.2,
    "audio_length": 5.4,
    "isStereo": false
  }
}
```

## API Reference

### Authentication

All requests require a Bearer token in the Authorization header:

```
Authorization: Bearer YOUR_API_KEY
```

### Core Endpoints

#### 🎯 Speech Transcription

```
POST /api/transcribe/speech
```

**Parameters:**

- `mode` (optional): `fulltext`, `quick`, `diarization`, `quick-diarization`
- `model` (optional): `apifant` (default)
- `source_lang` (optional): Source language code
- `target_lang` (optional): Target language code
- `custom_vocabularies` (optional): Custom words separated by commas
- `no_word_timestamps` (optional): Skip word timestamps for better performance

#### 🔍 Language Detection

```
POST /api/transcribe/detect/language
```

Automatically detect the spoken language in your audio file.

#### 🔄 Batch Processing

```
POST /api/batch/transcribe/fulltext
GET /api/batch/transcribe/retrieve/{transcription_id}
```

Process large files asynchronously with webhook notifications.

#### 🌐 Webhook Integration

```
POST /api/transcribe/speech/webhook
```

Real-time processing with results delivered to your webhook URL.

## Supported Languages

| Language | Code |
|----------|------|
| English | `en` |
| German | `de` |
| Spanish | `es` |
| French | `fr` |
| Italian | `it` |
| Portuguese | `pt` |
| Dutch | `nl` |
| Russian | `ru` |
| Chinese | `zh` |
| Japanese | `ja` |
| Korean | `ko` |
| Arabic | `ar` |
| Polish | `pl` |
| Romanian | `ro` |
| Czech | `cs` |
| Turkish | `tr` |
| Ukrainian | `uk` |

## Transcription Modes

### 📝 Full Text Mode

- **Use Case**: Complete, accurate transcription
- **Features**: Word timestamps, speaker identification, full sentences
- **Best For**: Meetings, interviews, lectures

### ⚡ Quick Mode

- **Use Case**: Fast transcription with good accuracy
- **Features**: Optimized for speed, basic speaker detection
- **Best For**: Real-time applications, quick previews

### 👥 Diarization Mode

- **Use Case**: Multi-speaker audio with speaker separation
- **Features**: Advanced speaker identification, conversation flow
- **Best For**: Conference calls, panel discussions
- **Note**: Requires mono audio files

<!-- ## 🛠️ SDKs & Libraries

We provide official SDKs for popular programming languages:

- **TypeScript/JavaScript**: [apifant-phone-sdk-typescript](https://github.com/apifant/apifant-phone-sdk-typescript)
- **C#/.NET**: [apifant-transcription-sdk-csharp](https://github.com/apifant/apifant-transcription-sdk-csharp)
- **Java**: [apifant-transcription-sdk-java](https://github.com/apifant/apifant-transcription-sdk-java)
- **Python**: [apifant-transcription-sdk-python](https://github.com/apifant/apifant-transcription-sdk-python) -->

## Performance & Limits

### File Limits

- **Maximum file size**: 300MB
- **Supported formats**: MP3, WAV
- **Recommended**: Clear audio, minimal background noise

### Rate Limits

- **Concurrent requests**: 10 per API key
- **Daily quota**: Based on your subscription plan

## Webhook Integration

For batch processing, configure webhooks to receive real-time updates:

```json
{
  "transcription_id": "uuid-string",
  "status": "completed",
  "timestamp": 1640995200,
  "result": {
    "full_text": "Transcribed content...",
    "word_count": 150,
    "sentence_count": 8,
    "target_language": "en",
    "source_language": "en"
  }
}
```

### Webhook Authentication

- **x-api-key**: Include your API key in the `X-API-Key` header
- **bearer**: Use Bearer token authentication

## Security & Privacy

- **🔐 Encrypted transmission**: All API calls use HTTPS
- **🗑️ No data retention**: Audio files are deleted after processing

## Code Examples

### JavaScript Example

```javascript
const fs = require('fs');
const fetch = require('node-fetch');

const transcribeAudio = async () => {
  const audioBuffer = fs.readFileSync('audio.wav');
  
  const response = await fetch('https://your-endpoint.apifant.com/api/transcribe/speech?mode=fulltext&source_lang=en', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer YOUR_API_KEY',
      'Content-Type': 'audio/wav'
    },
    body: audioBuffer
  });
  
  const result = await response.json();
  console.log(result.full_text);
};

transcribeAudio();
```

### cURL Example with Custom Vocabulary

```bash
curl -X POST \
  'https://your-endpoint.apifant.com/api/transcribe/speech?mode=fulltext&source_lang=en&custom_vocabularies=Apifant,API,transcription' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'Content-Type: audio/wav' \
  --data-binary '@technical-meeting.wav'
```

## Error Handling

### Common HTTP Status Codes

- **200**: Success
- **400**: Bad Request - Invalid parameters
- **401**: Unauthorized - Invalid API key
- **413**: Payload Too Large - File exceeds 300MB
- **429**: Too Many Requests - Rate limit exceeded
- **500**: Internal Server Error

### Error Response Format

```json
{
  "error": "Invalid audio format",
  "code": "INVALID_FORMAT",
  "details": "Supported formats: MP3, WAV"
}
```

## Monitoring & Analytics

### Health Check

```
GET /api/health
```

### Version Information

```
GET /api/version
```

## Use Cases

### 📞 Call Center Analytics

- **Transcribe customer calls**
- **Sentiment analysis**
- **Quality assurance**
- **Compliance monitoring**

### 📱 Mobile Applications

- **Voice-to-text input**
- **Real-time transcription**
- **Accessibility features**
- **Voice commands**

## 📞 Support & Resources

- **📧 Email**: [support@apifant.com](mailto:support@apifant.com)
- **📚 Documentation**: [https://docs.apifant.com](https://docs.apifant.com)
- **🐛 Bug Reports**: [GitHub Issues](https://github.com/apifant/apifant-transcription-api/issues)

## 📄 License

This API is provided under the [Apifant Terms of Service](https://apifant.com/terms).
