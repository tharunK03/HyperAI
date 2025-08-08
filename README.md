# AI Teaching Assistant Platform

A comprehensive AI-powered teaching platform that helps educators scale their teaching capabilities with intelligent feedback, video processing, and interactive learning experiences.

## 🚀 Features

### Core Functionality
- **AI-Powered Code Feedback**: Intelligent code evaluation with timestamp-based video references
- **Video Processing**: Automatic keyframe extraction, OCR, and transcription
- **Interactive Learning**: Real-time code execution and feedback
- **Multi-Language Support**: Python, JavaScript, SQL, and more
- **User Management**: Google OAuth authentication with role-based access

### Video Context Integration
- **Smart Timestamp References**: When students submit incorrect code, the system provides specific video timestamps explaining the topic they got wrong
- **Dynamic Feedback**: Feedback includes references like "(refer to 0:16 for correct sorting data)"
- **OCR Processing**: Extracts text from video keyframes for context-aware feedback
- **Audio Transcription**: Whisper-based audio processing for comprehensive video understanding

## 🏗️ Architecture

### Backend (FastAPI)
```
sensai-ai/
├── src/
│   ├── api/
│   │   ├── routes/          # API endpoints
│   │   ├── db/             # Database operations
│   │   ├── utils/          # Video processing, LLM integration
│   │   └── models.py       # Pydantic models
│   └── main.py             # FastAPI application
```

### Frontend (Next.js)
```
sensai-frontend/
├── src/
│   ├── app/                # Next.js app router
│   ├── components/         # React components
│   ├── lib/               # Utility functions
│   └── types/             # TypeScript definitions
```

## 🛠️ Technology Stack

### Backend
- **FastAPI**: Modern Python web framework
- **SQLite**: Lightweight database
- **OpenAI/Gemini**: LLM integration for feedback generation
- **OpenCV**: Video processing and keyframe extraction
- **Whisper**: Audio transcription
- **PyTesseract**: OCR for video keyframes
- **MoviePy**: Video manipulation

### Frontend
- **Next.js 14**: React framework with app router
- **TypeScript**: Type-safe development
- **Tailwind CSS**: Utility-first styling
- **Shadcn UI**: Component library
- **Monaco Editor**: Code editor integration

## 📦 Installation

### Prerequisites
- Python 3.12+
- Node.js 18+
- Tesseract OCR: `brew install tesseract` (macOS)
- FFmpeg: `brew install ffmpeg` (macOS)

### Backend Setup
```bash
cd sensai-ai
python -m venv myenv
source myenv/bin/activate  # On Windows: myenv\Scripts\activate
pip install -r requirements.txt

# Initialize database
cd src
python init_db.py

# Start backend server
uvicorn main:app --reload --port 8000
```

### Frontend Setup
```bash
cd sensai-frontend
npm install

# Create environment file
cp .env.example .env.local
# Update NEXT_PUBLIC_BACKEND_URL=http://localhost:8000

# Start development server
npm run dev
```

## 🔧 Configuration

### Environment Variables

#### Backend (.env)
```bash
OPENAI_API_KEY=your_openai_key
GEMINI_API_KEY=your_gemini_key
GOOGLE_CLIENT_ID=your_google_client_id
```

#### Frontend (.env.local)
```bash
NEXT_PUBLIC_BACKEND_URL=http://localhost:8000
NEXTAUTH_SECRET=your_nextauth_secret
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
```

## 🎯 Key Features Explained

### Video Processing Pipeline
1. **Upload**: Users upload videos with question associations
2. **Processing**: 
   - Keyframe extraction using OpenCV
   - OCR text extraction from keyframes
   - Audio transcription using Whisper
   - Metadata storage in SQLite
3. **Context Integration**: Video context is linked to questions for intelligent feedback

### AI Feedback Generation
1. **Code Submission**: Students submit code through the playground
2. **Context Retrieval**: System fetches video context and keyframes
3. **LLM Processing**: OpenAI/Gemini generates feedback with timestamp references
4. **Dynamic References**: When code is incorrect, feedback includes specific video timestamps

### Example Feedback Output
```json
{
  "logic": {
    "score": 1,
    "comment": "The code is completely incorrect. Please refer to the video at 04:01 for a complete algorithm implementation. The video at 00:18 explains the core principles of quicksort.",
    "evidence_lines": [1, 2]
  }
}
```

## 🗄️ Database Schema

### Core Tables
- `questions`: Learning questions with video context
- `video_context`: Processed video data with keyframes
- `submissions`: Student code submissions
- `feedback`: AI-generated feedback
- `users`: User management
- `organizations`: Multi-tenant support

### Video Context Structure
```sql
CREATE TABLE video_context (
    id INTEGER PRIMARY KEY,
    question_id TEXT,
    video_file_path TEXT,
    video_hash TEXT,
    transcript TEXT,
    keyframes_data TEXT,  -- JSON array of keyframes with timestamps
    duration REAL,
    processed_at TIMESTAMP
);
```

## 🚀 API Endpoints

### Video Processing
- `POST /video/upload`: Upload and process videos
- `GET /video/context/{question_id}`: Get video context for questions
- `GET /video/timestamps/{question_id}`: Get available timestamps

### Code Feedback
- `POST /feedback/submit`: Submit code for AI feedback
- `POST /code/submit`: Legacy endpoint (now redirects to feedback)

### Code Execution
- `POST /api/code/submit`: Execute code using Judge0
- `GET /api/code/status`: Check execution status

## 🧪 Testing

### Backend Tests
```bash
cd sensai-ai/src
pytest tests/
```

### Frontend Tests
```bash
cd sensai-frontend
npm test
```

## 🔍 Debugging

### Common Issues
1. **Tesseract not found**: Install with `brew install tesseract`
2. **Database errors**: Run `python init_db.py` to recreate tables
3. **Video processing fails**: Ensure FFmpeg is installed
4. **LLM errors**: Check API keys in environment variables

### Logging
- Backend logs include detailed video processing information
- Frontend console shows API request/response cycles
- Database operations are logged for debugging

## 📈 Performance

### Video Processing
- Keyframe extraction: ~1-2 seconds per minute of video
- OCR processing: ~0.5 seconds per keyframe
- Audio transcription: ~2-3x real-time

### Feedback Generation
- Average response time: 2-5 seconds
- Supports streaming responses for better UX
- Automatic retry logic with exponential backoff

## 🔒 Security

- Google OAuth for authentication
- API key management for LLM services
- Input validation and sanitization
- CORS configuration for frontend-backend communication

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For issues and questions:
1. Check the debugging section above
2. Review the logs for error details
3. Ensure all dependencies are properly installed
4. Verify environment variables are correctly set

---

**Note**: This platform is designed for educational use and includes advanced AI features for personalized learning experiences. The video processing and timestamp reference system provides students with targeted guidance when they encounter difficulties with coding problems. 