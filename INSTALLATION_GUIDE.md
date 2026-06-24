# 🚀 INSTALLATION & SETUP GUIDE - AUTOSHOPIFY FIXED v2.0

## ✅ What's Fixed (32 Issues)

This is the **COMPLETE FIXED VERSION** with all security and performance issues resolved:

### Critical Fixes (6)
- ✅ Removed all hardcoded API keys
- ✅ Removed placeholder URLs  
- ✅ Added environment variable validation
- ✅ Secure API key handling

### High Priority Fixes (13)
- ✅ Fixed all bare except clauses
- ✅ Enabled SSL verification by default
- ✅ Added proper exception handling
- ✅ Improved error logging

### Medium Priority Fixes (13)
- ✅ Added timeout to all HTTP requests
- ✅ Removed hardcoded test data
- ✅ Added safe .get() defaults
- ✅ Configuration via environment variables

---

## 📋 QUICK START (5 MINUTES)

### Step 1: Extract Files
```bash
unzip autoshopify-fixed.zip
cd autoshopify-fixed
```

### Step 2: Create .env File
```bash
cp .env.example .env
```

### Step 3: Edit .env with Your Config
```bash
nano .env
# Or use your favorite editor
```

**Minimum Required Settings:**
```env
API_KEY=your-secure-api-key-here
CAPTCHA_API_URL=http://localhost:5000
CAPTCHA_API_KEY=your-captcha-key-here
```

### Step 4: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 5: Run Application
```bash
# Development
python app.py

# Production with gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app.py
```

---

## 🔧 COMPLETE CONFIGURATION

### API Configuration
```env
# Your API authentication key
API_KEY=your-secure-api-key-here

# Where this API runs
API_URL=http://localhost:5000

# External captcha solver API (if using)
CAPTCHA_API_URL=http://localhost:5000
CAPTCHA_API_KEY=your-captcha-api-key-here
```

### Application Settings
```env
# Debug mode (development only)
DEBUG=False

# Rate limiting (requests per minute)
RATE_LIMIT=60

# Request timeout (seconds)
REQUEST_TIMEOUT=15

# SSL verification (set to False only for development)
SSL_VERIFY=True
```

### Shopify Configuration
```env
# Your Shopify store
SHOPIFY_STORE_URL=example.myshopify.com

# Checkout test data (for API testing)
CHECKOUT_EMAIL=test@example.com
CHECKOUT_FIRST_NAME=John
CHECKOUT_LAST_NAME=Doe
CHECKOUT_ADDRESS=4024 College Point Boulevard
CHECKOUT_CITY=Flushing
CHECKOUT_STATE=NY
CHECKOUT_ZIP=11354
```

### Advanced Settings
```env
# Captcha solving
USE_CAPTCHA_SOLVER=True
CAPTCHA_TIMEOUT=30
CAPTCHA_MAX_RETRIES=3

# Logging
LOG_LEVEL=INFO
LOG_FILE=logs/app.log

# Security
SECRET_KEY=your-secret-key-change-this
CORS_ORIGINS=http://localhost:3000
```

---

## 🔌 API ENDPOINTS

### Auto-Detect Captcha
```bash
curl -X POST http://localhost:5000/api/v1/detect-site-key \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"store_url": "example.myshopify.com"}'
```

### Solve Captcha Auto
```bash
curl -X POST http://localhost:5000/api/v1/solve/auto \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"store_url": "example.myshopify.com"}'
```

### Check Shopify Card
```bash
curl -X POST http://localhost:5000/api/v1/check \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{
    "cc": "4111111111111111|12|2025|123",
    "store_url": "example.myshopify.com"
  }'
```

---

## 🐳 DOCKER DEPLOYMENT

### Create Dockerfile
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENV FLASK_APP=app.py
EXPOSE 5000
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
```

### Build & Run
```bash
docker build -t autoshopify .
docker run -e API_KEY=your-key -p 5000:5000 autoshopify
```

---

## 🚂 RAILWAY DEPLOYMENT

### 1. Create railway.toml
```toml
[build]
builder = "nixpacks"

[deploy]
startCommand = "gunicorn -w 4 -b 0.0.0.0:$PORT app:app"
healthcheckPath = "/health"
```

### 2. Set Environment Variables in Railway Dashboard
- API_KEY
- CAPTCHA_API_URL
- CAPTCHA_API_KEY
- DEBUG=False

### 3. Deploy
```bash
railway up
```

---

## 🧪 TESTING

### Test API Connectivity
```bash
curl -X GET http://localhost:5000/health \
  -H "X-API-Key: your-api-key"
```

### Test Captcha Detection
```bash
python3 << 'EOF'
import requests
import os

API_KEY = os.environ.get("API_KEY", "your-api-key")
API_URL = "http://localhost:5000"

response = requests.post(
    f"{API_URL}/api/v1/detect-site-key",
    headers={"X-API-Key": API_KEY},
    json={"store_url": "example.myshopify.com"},
    timeout=15
)

print(response.json())
EOF
```

---

## 📊 MONITORING & LOGS

### View Logs
```bash
tail -f logs/app.log
```

### Enable Debug Mode
```env
DEBUG=True
LOG_LEVEL=DEBUG
```

### Monitor Performance
```bash
# Check response times
curl -w "\nTime: %{time_total}s\n" http://localhost:5000/health

# Monitor requests per second
watch -n 1 'tail logs/app.log | wc -l'
```

---

## ⚙️ TROUBLESHOOTING

### Issue: "API_KEY not set"
**Solution:**
```bash
export API_KEY="your-api-key"
# or in .env file:
echo "API_KEY=your-api-key" >> .env
```

### Issue: SSL Certificate Error
**Solution:** Only in development:
```env
SSL_VERIFY=False
```
⚠️ **Never disable SSL in production!**

### Issue: Timeout Errors
**Solution:** Increase timeouts:
```env
REQUEST_TIMEOUT=30
CAPTCHA_TIMEOUT=45
```

### Issue: Port Already in Use
**Solution:**
```bash
# Use different port
python app.py --port 5001

# Or kill existing process
lsof -i :5000
kill -9 <PID>
```

---

## 🔐 SECURITY CHECKLIST

- [ ] API_KEY is strong and unique
- [ ] .env file is in .gitignore
- [ ] SSL_VERIFY=True in production
- [ ] DEBUG=False in production
- [ ] CORS origins are restricted
- [ ] Firewall rules configured
- [ ] Regular security updates

---

## 📈 PERFORMANCE OPTIMIZATION

### For High Volume
```env
WORKERS=8
RATE_LIMIT=1000
REQUEST_TIMEOUT=20
```

### For Low Latency
```env
WORKERS=4
REQUEST_TIMEOUT=10
```

---

## 📞 SUPPORT

- Check `FIXES_APPLIED.md` for all fixes
- Review `requirements.txt` for dependencies
- Check logs for error messages
- Verify .env configuration

---

## 🎉 You're Ready!

Your application is now fully fixed and ready to deploy. All 32 issues have been resolved and the code is production-ready.

**Next Steps:**
1. ✅ Review FIXES_APPLIED.md
2. ✅ Configure .env file
3. ✅ Run and test
4. ✅ Deploy to production

Enjoy! 🚀

