# RAG with History - Conversational PDF Q&A

A Streamlit-based conversational AI application that enables users to upload PDF documents and ask questions about their content while maintaining chat history across the conversation.

## Features

- 📄 **PDF Upload**: Upload single or multiple PDF files
- 💬 **Conversational AI**: Ask questions about your documents in natural language
- 🧠 **Chat History**: Maintains conversation context across multiple questions
- 🔍 **RAG Pipeline**: Uses Retrieval Augmented Generation for accurate answers
- ⚡ **Efficient Processing**: Caches embeddings and vectorstore to avoid reprocessing
- 🎯 **Context-Aware**: Reformulates questions based on chat history for better responses

## Technologies Used

- **Streamlit**: Web application framework
- **LangChain**: Framework for building LLM applications
- **Groq**: Fast LLM inference (llama-3.3-70b-versatile)
- **HuggingFace Embeddings**: all-MiniLM-L6-v2 model for document embeddings
- **Chroma**: Vector database for document storage and retrieval
- **PyPDF**: PDF document processing


**Using the app**:
   - Check the "Ready to start?" checkbox
   - Enter a session ID (or use the default)
   - Upload one or more PDF files using the file uploader
   - Wait for "Documents processed and vectorstore created!" message
   - Type your questions in the text input
   - The AI will answer based on the uploaded document content

## How It Works

### 1. Document Processing
- PDFs are uploaded and temporarily saved
- Documents are loaded using PyPDFLoader
- Text is split into chunks (5000 chars with 500 overlap)
- Embeddings are created using HuggingFace's all-MiniLM-L6-v2

### 2. Vector Storage
- Document chunks are stored in Chroma vector database
- Vectorstore is cached in session state to avoid reprocessing
- Only recreates when different files are uploaded

### 3. Conversational RAG
- **History-Aware Retrieval**: Reformulates questions based on chat history
- **Context Retrieval**: Retrieves relevant document chunks
- **Answer Generation**: LLM generates concise answers using retrieved context
- **Chat History**: Maintains conversation context per session

### 4. Session Management
- Each session ID maintains its own chat history
- Multiple users can use the app simultaneously with different sessions
- History persists throughout the session

## Project Structure

```
RAG_with_History/
├── app.py                 # Main Streamlit application
├── README.md             # This file
└── .env                  # Environment variables (not tracked)
```

## Configuration

### Chunk Settings
- **Chunk Size**: 5000 characters
- **Chunk Overlap**: 500 characters

### Model Settings
- **LLM**: Groq's llama-3.3-70b-versatile
- **Embeddings**: all-MiniLM-L6-v2 (HuggingFace)
- **Max Answer Length**: 3 sentences (concise responses)

## Key Features Explained

### Caching for Performance
The app caches the vectorstore and retriever in `st.session_state` to prevent unnecessary reprocessing on every interaction. This significantly improves response time and reduces computational overhead.

### History-Aware Retrieval
The system reformulates user questions based on previous conversation context, allowing for follow-up questions like "What about that?" or "Tell me more" to work correctly.

### Session Management
Each session maintains its own chat history, allowing multiple independent conversations or testing different approaches with the same documents.

### Groq API errors
- Verify your API key in `.env`
- Check your Groq API quota/limits
- Ensure you have internet connection

### Memory issues with large PDFs
- Try reducing chunk_size
- Process fewer PDFs at once
- Use a machine with more RAM

## Future Enhancements

- [ ] Support for more document formats (DOCX, TXT, etc.)
- [ ] Export chat history
- [ ] Multiple LLM provider options
- [ ] Document source citations in answers
- [ ] User authentication
- [ ] Persistent storage across sessions

