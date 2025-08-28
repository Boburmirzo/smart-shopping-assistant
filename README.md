# Smart Shopping Assistant Backend

A powerful shopping assistant **backend API** powered by **[Swarms](https://github.com/kyegomez/swarms)** AI framework and **[Memori](https://github.com/memorisdk/memori-python)** for persistent memory.

## 🌟 Features

- **🤖 AI-Powered Assistant**: Intelligent shopping assistant with personalized recommendations
- **🧠 Memory-Enhanced Shopping**: Remembers customer preferences and shopping history using Memori
- **🔌 RESTful API**: FastAPI backend with comprehensive endpoints
- **💾 Persistent Memory**: Customer interactions stored and recalled across sessions
- **🚀 Simple Setup**: Just OpenAI API key required!
- **📡 Ready for Integration**: Connect any frontend (React, Vue, Mobile app, etc.)

## 🏗️ Architecture

```
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│                     │    │                     │    │                     │
│   Frontend Client   │    │    FastAPI          │    │   Swarms AI         │
│ (Your Application)  │◄───┤    Backend          │◄───┤   Agent System      │
│                     │    │                     │    │                     │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
                                      │                          │
                                      ▼                          ▼
                           ┌─────────────────────┐    ┌─────────────────────┐
                           │                     │    │  Personal Shopping  │
                           │   Memori Memory     │◄───┤     Assistant       │
                           │   System (SQLite)   │    │      Agent          │
                           │                     │    │                     │
                           └─────────────────────┘    └─────────────────────┘
```

## 📁 Project Structure

```
smart-shopping-assistant/
├── backend/
│   ├── main.py                 # FastAPI application with Swarms agent
│   ├── requirements.txt        # Python dependencies
│   ├── .env.example           # Environment configuration template
│   └── vercel.json            # Vercel deployment configuration
└── README.md                   # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8+ 
- OpenAI API key

### Setup

```bash
# Navigate to the backend directory
cd backend

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your OpenAI API key:
# OPENAI_API_KEY=your-openai-api-key-here

# Start the FastAPI server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Access the API

- **Backend API**: http://localhost:8000
- **API Documentation**: http://localhost:8000/docs
- **Interactive API Explorer**: http://localhost:8000/redoc

## 🔧 Configuration

### Environment Setup

Create a `.env` file in the `backend` directory:

```env
OPENAI_API_KEY=your-openai-api-key-here
```

### Optional Configuration

```env
# Custom OpenAI settings (optional)
OPENAI_MODEL=gpt-4
OPENAI_BASE_URL=https://api.openai.com/v1

# Database settings (optional)
DATABASE_PATH=sqlite:///smart_shopping_swarms.db
NAMESPACE=smart_shopping_swarms
```

### Memory System

The Memori system automatically creates a SQLite database (`smart_shopping_swarms.db`) to store conversation history and customer preferences. No additional setup required.

## 📡 API Endpoints

### Chat
- `POST /chat` - Send message to AI assistant
  ```json
  {
    "message": "I need a laptop for programming",
    "customer_id": "customer_123"
  }
  ```

### Products
- `GET /products` - Get all products
- `POST /products/search` - Search products with filters
- `GET /products/{id}` - Get specific product
- `GET /categories` - Get product categories

### Memory
- `GET /memory/search?query=laptop` - Search customer memory (admin)

### Health Check
- `GET /` - API health check

## � Frontend Integration

This backend is designed to work with any frontend framework. Here are some examples:

### React/Next.js
```javascript
### React/Next.js Example
```javascript
const response = await fetch('/api/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    message: "Show me laptops under $1000",
    customer_id: "user_123"
  })
});
const data = await response.json();
```

### Python Client Example
```python
import requests

response = requests.post('/api/chat', json={
    "message": "I need a smartphone with good camera",
    "customer_id": "user_456"
})
print(response.json())
```

### cURL Example
```bash
curl -X POST "/api/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "What tablets do you have?", "customer_id": "user_789"}'
```

## 🔍 How It Works

1. **Customer Interaction**: User chats with the AI assistant about shopping needs
2. **AI Processing**: Swarms agent analyzes the request with shopping expertise
3. **Memory Search**: Agent searches previous interactions for context
4. **Personalized Response**: AI suggests relevant products based on memory and expertise
5. **Memory Storage**: Conversation automatically stored in Memori for future reference

## 🛠️ Development

### Testing the API

```bash
# Test the API endpoints
curl http://localhost:8000/
curl http://localhost:8000/products

# Test chat functionality
curl -X POST "/api/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello", "customer_id": "test"}'
```

### Customizing the Product Catalog

Edit the `PRODUCT_CATALOG` in `main.py` to add your own products with:
- Product information
- Images
- Categories  
- Descriptions

### Adding New Endpoints

1. **Add new routes** in `main.py`
2. **Define request/response models** using Pydantic
3. **Update API documentation** (automatic with FastAPI)
4. **Test endpoints** using the interactive docs at `/docs`

## 🚀 Deployment

### Local Development
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Production with Gunicorn
```bash
pip install gunicorn
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

### Docker Deployment
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Vercel Deployment
The project includes `vercel.json` for easy deployment to Vercel:
```bash
npm i -g vercel
vercel --prod
```

## 🐛 Troubleshooting

### Backend Issues
- **Service not initialized**: Check OpenAI API key in `.env` file
- **Memory errors**: Ensure SQLite database directory is writable
- **CORS errors**: Verify frontend URL in CORS middleware
- **Agent errors**: Check OpenAI API key and rate limits
- **Import errors**: Ensure `pip install -r requirements.txt` completed successfully

### API Testing
```bash
# Check if server is running
curl http://localhost:8000/

# Test with verbose output
curl -v http://localhost:8000/products

# Check logs in terminal where uvicorn is running
```

### Common Issues
- **Agent initialization**: Check OpenAI API key is valid and has sufficient credits
- **Memory integration**: Verify database permissions and file system access
- **Performance**: Monitor OpenAI API usage and rate limits

## ⚡ Performance

- **Fast responses**: Direct OpenAI integration
- **Smart memory**: Automatic context retrieval and integration
- **Scalable**: Simple to deploy and scale
- **Cost efficient**: No additional platform fees
- **Frontend agnostic**: Works with any client application

## 📝 License

This project is open source. Please refer to the license file for usage terms.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Test API endpoints with curl or your preferred client
4. Make your changes
5. Test thoroughly
6. Submit a pull request

## 📞 Support

For issues related to:
- **Memori**: Check the [Memori documentation](https://github.com/memorisdk/memori-python)
- **Swarms**: Consult [Swarms documentation](https://github.com/kyegomez/swarms)
- **FastAPI**: See [FastAPI documentation](https://fastapi.tiangolo.com/)
- **This Backend**: Create an issue in the repository

---

**Smart Shopping Assistant Backend - Powerful AI assistant ready for any frontend! 🛍️🤖✨**
```

### Python Client
```python
import requests

class ShoppingAssistant:
    def __init__(self, base_url="http://localhost:8000"):
        self.base_url = base_url
    
    def chat(self, message, customer_id):
        response = requests.post(f"{self.base_url}/chat", json={
            "message": message,
            "customer_id": customer_id
        })
        return response.json()
```

### Mobile App (React Native)
```javascript
const useShoppingAssistant = () => {
  const chat = async (message, customerId) => {
    const response = await fetch('http://your-api.com/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ message, customer_id: customerId })
    });
    return response.json();
  };
  return { chat };
};
```ntic
3. **Update API documentation** (automatic with FastAPI)
4. **Test endpoints** using the interactive docs at `/docs`

### Customizing the Product Catalog

Edit the `PRODUCT_CATALOG` in `main.py` to add your own products with:
- Product information
- Images
- Categories  
- Descriptions

## 🚀 Deployment

### Local Development
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Production with Gunicorn
```bash
pip install gunicorn
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

### Docker Deployment
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Vercel Deployment
The project includes `vercel.json` for easy deployment to Vercel:
```bash
npm i -g vercel
vercel --prod
```for persistent memory. 

## 🌟 Features

- **🤖 Multi-Agent Intelligence**: Specialized AI agents (Personal Shopper + Product Expert)
- **🧠 Memory-Enhanced Shopping**: Remembers customer preferences and shopping history using Memori
- **🎯 Intelligent Collaboration**: Agents automatically work together for better responses
- **🔌 RESTful API**: FastAPI backend with comprehensive endpoints
- **💾 Persistent Memory**: Customer interactions stored and recalled across sessions
- **🚀 Simple Setup**: Just OpenAI API key required!
- **📡 Ready for Integration**: Connect any frontend (React, Vue, Mobile app, etc.)

## 🏗️ Architecture

```
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│                     │    │                     │    │                     │
│   Frontend Client   │    │    FastAPI          │    │   Swarms Multi-     │
│ (Your Application)  │◄───┤    Backend          │◄───┤   Agent System      │
│                     │    │                     │    │                     │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
                                      │                          │
                                      ▼                          ▼
                           ┌─────────────────────┐    ┌─────────────────────┐
                           │                     │    │  Personal Shopper   │
                           │   Memori Memory     │◄───┤  + Product Expert   │
                           │   System (SQLite)   │    │     Agents          │
                           │                     │    │                     │
                           └─────────────────────┘    └─────────────────────┘
```

## 🤖 Agent System

### Personal Shopping Assistant
- **Role**: Primary customer interface and service
- **Capabilities**: Customer preferences, recommendations, budget analysis
- **Memory**: Automatically searches past interactions for personalization

### Product Expert  
- **Role**: Technical product specialist and comparison expert
- **Capabilities**: Detailed specs, comparisons, value analysis
- **Memory**: Remembers customer's technical preferences and requirements

## 📁 Project Structure

```
smart-shopping-assistant/
├── backend/
│   ├── main.py                 # FastAPI application with Swarms agents
│   ├── requirements.txt        # Python dependencies
│   ├── .env.example           # Environment configuration template
│   └── vercel.json            # Vercel deployment configuration
└── README.md                   # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8+ 
- OpenAI API key

### Setup

```bash
# Clone or navigate to the backend directory
cd backend

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your OpenAI API key:
# OPENAI_API_KEY=your-openai-api-key-here

# Start the FastAPI server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Access the API

- **Backend API**: http://localhost:8000
- **API Documentation**: http://localhost:8000/docs
- **Interactive API Explorer**: http://localhost:8000/redoc

## 🔧 Configuration

### Simple Setup

Just one environment variable needed in `backend/.env`:

```env
OPENAI_API_KEY=your-openai-api-key-here
```

### Optional Configuration

```env
# Custom OpenAI settings (optional)
OPENAI_MODEL=gpt-4
OPENAI_BASE_URL=https://api.openai.com/v1

# Database settings (optional)
DATABASE_PATH=sqlite:///smart_shopping_swarms.db
NAMESPACE=smart_shopping_swarms
```

### Memory System

The Memori system automatically creates a SQLite database (`smart_shopping_swarms.db`) to store conversation history and customer preferences. No additional setup required.

## 📡 API Endpoints

### Chat
- `POST /chat` - Send message to AI assistant
  ```json
  {
    "message": "I need a laptop for programming",
    "customer_id": "customer_123"
  }
  ```

### Products
- `GET /products` - Get all products
- `POST /products/search` - Search products with filters
- `GET /products/{id}` - Get specific product
- `GET /categories` - Get product categories

### Memory
- `GET /memory/search?query=laptop` - Search customer memory (admin)

### Health Check
- `GET /` - API health check

## 💻 Frontend Integration

This backend is designed to work with any frontend framework. Here are some examples:

### React/Next.js Example
```javascript
const response = await fetch('http://localhost:8000/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    message: "Show me laptops under $1000",
    customer_id: "user_123"
  })
});
const data = await response.json();
```

### Python Client Example
```python
import requests

response = requests.post('http://localhost:8000/chat', json={
    "message": "I need a smartphone with good camera",
    "customer_id": "user_456"
})
print(response.json())
```

### cURL Example
```bash
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "What tablets do you have?", "customer_id": "user_789"}'
```

## 🔍 How It Works

1. **Customer Interaction**: User chats with the AI assistant about shopping needs
2. **Agent Coordination**: Personal Shopper agent analyzes the request
3. **Automatic Memory Search**: Agents search previous interactions for context using built-in functions
4. **Expert Collaboration**: Product Expert agent provides technical insights when needed
5. **AI Processing**: Swarms orchestrates multi-agent collaboration for enhanced responses
6. **Memory Storage**: Conversation automatically stored in Memori for future reference
7. **Personalized Recommendations**: AI suggests relevant products based on memory and expertise

## 🛠️ Development

### Testing the API

```bash
# Test the API endpoints
curl http://localhost:8000/
curl http://localhost:8000/products

# Test chat functionality
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello", "customer_id": "test"}'
```

### Adding New Agents

```python
# Create a new specialized agent
new_agent = Agent(
    agent_name="Price Comparison Expert",
    system_prompt="You are an expert at finding the best deals...",
    functions=[search_memory],
    max_loops=2,
)

# Add to the crew
shopping_assistant_crew = AgentRearrange(
    agents=[personal_shopper, product_expert, new_agent],
    flow="Personal Shopping Assistant -> Product Expert -> Price Comparison Expert",
)
```

### Customizing Agent Behavior

Edit agent system prompts in `main.py`:
- `Personal Shopping Assistant`: Customer service and recommendations
- `Product Expert`: Technical knowledge and comparisons

### Adding New Features

1. **Backend**: Add new endpoints in `main.py`
2. **Frontend**: Create new components in `src/components/`
3. **API Integration**: Update `src/lib/api.ts`
4. **Agents**: Modify agent prompts or add new specialized agents

### Customizing the Product Catalog

Edit the `PRODUCT_CATALOG` in `backend/main.py` to add your own products with:
- Product information
- Images
- Categories  
- Descriptions

## 🐛 Troubleshooting

### Backend Issues
- **Service not initialized**: Check OpenAI API key in `.env` file
- **Memory errors**: Ensure SQLite database directory is writable
- **CORS errors**: Verify frontend URL in CORS middleware
- **Agent errors**: Check OpenAI API key and rate limits
- **Import errors**: Ensure `pip install swarms memorisdk` completed successfully

### API Testing
```bash
# Check if server is running
curl http://localhost:8000/

# Test with verbose output
curl -v http://localhost:8000/products

# Check logs
tail -f server.log
```

### Swarms Integration Issues
- **Agent initialization**: Check OpenAI API key is valid and has sufficient credits
- **Memory integration**: Verify database permissions and file system access
- **Performance**: Monitor OpenAI API usage and rate limits

## � Migration from DigitalOcean

If upgrading from the previous DigitalOcean implementation:

1. **Update dependencies**: `pip install swarms` (remove `openai` if not needed elsewhere)
2. **Update environment**: Remove `agent_endpoint` and `agent_access_key`, keep only `OPENAI_API_KEY`
3. **Database migration**: The new system creates `smart_shopping_swarms.db` (old data in separate DB)
4. **No frontend changes**: API interface remains identical
5. **Test thoroughly**: Run `python test_swarms_integration.py`

## ⚡ Performance Benefits

- **Faster responses**: Direct OpenAI integration
- **Better accuracy**: Specialized agent roles and collaboration
- **Enhanced memory**: Automatic context retrieval and integration
- **Reduced complexity**: 75% fewer lines of code
- **Cost efficiency**: No additional platform fees
- **Easier scaling**: Simple to add more specialized agents

## �📝 License

This project is part of the Memori demonstration suite. Please refer to the main Memori license for usage terms.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Test API endpoints with curl or your preferred client
4. Make your changes
5. Test thoroughly
6. Submit a pull request

## 📞 Support

For issues related to:
- **Memori**: Check the main Memori documentation
- **Swarms**: Consult [Swarms documentation](https://github.com/kyegomez/swarms)
- **FastAPI**: See [FastAPI documentation](https://fastapi.tiangolo.com/)
- **This Backend**: Create an issue in the repository

---

**Smart Shopping Assistant Backend - Powerful AI agents ready for any frontend! 🛍️🤖✨**
