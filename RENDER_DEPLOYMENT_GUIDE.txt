# Render Deployment Guide - Inventory Management System

## 🚀 Quick Deployment Steps

### 1. Database Setup on Render

1. **Create MySQL Database:**
   - Go to [Render Dashboard](https://dashboard.render.com)
   - Click **New +** → **MySQL**
   - Choose your plan (Free tier available)
   - Set database name: `inventory_management`
   - Note down the connection details provided

2. **Database Connection Info:**
   ```
   External Database URL: mysql://username:password@host:port/database_name
   Internal Database URL: mysql://username:password@host:port/database_name
   Host: dpg-xxxxx-a.oregon-postgres.render.com
   Port: 3306
   Database: inventory_management
   Username: inventory_management_user
   Password: [generated password]
   ```

### 2. Import Database Schema

Connect to your Render MySQL database and run the schema:

```bash
mysql -h [your-render-host] -u [your-username] -p[your-password] [your-database] < database_schema.sql
```

### 3. Deploy Flask Application

1. **Connect GitHub Repository:**
   - In Render Dashboard, click **New +** → **Web Service**
   - Connect your GitHub account and select this repository
   - Choose branch: `main` (or your default branch)

2. **Configure Build Settings:**
   ```
   Environment: Python 3
   Build Command: pip install -r requirements.txt
   Start Command: gunicorn app:app --bind 0.0.0.0:$PORT
   ```

3. **Environment Variables:**
   Add these in Render's Environment section:
   ```
   DB_HOST=your-render-mysql-host
   DB_USER=your-render-mysql-user
   DB_PASSWORD=your-render-mysql-password
   DB_NAME=inventory_management
   DB_PORT=3306
   SECRET_KEY=your-super-secret-production-key
   DEBUG=False
   ```

### 4. Deploy & Access

1. Click **Create Web Service**
2. Wait for deployment (usually 2-5 minutes)
3. Your app will be available at: `https://your-app-name.onrender.com`

## 📋 Files Created for Deployment

### ✅ Procfile
```
web: gunicorn app:app --bind 0.0.0.0:$PORT
```

### ✅ requirements.txt (Updated)
Added `gunicorn==21.2.0` for production server

### ✅ .env.example
Template for environment variables - **DO NOT** commit real credentials

### ✅ app.py (Updated)
- Added `import os`
- Updated to use `PORT` environment variable from Render

## 🔧 API Endpoints

Your deployed API will be available at:
- Base URL: `https://your-app-name.onrender.com`
- API Base: `https://your-app-name.onrender.com/api/v1`

### Main Endpoints:
- **Vendors:** `/api/v1/vendors`
- **Warehouses:** `/api/v1/warehouses`
- **Products:** `/api/v1/products`
- **Shipments:** `/api/v1/shipments`
- **Orders:** `/api/v1/orders`
- **Analytics:** `/api/v1/analytics/dashboard`
- **Forecasting:** `/api/v1/forecast/generate`
- **Health Check:** `/api/v1/health`

## 🛠️ Production Considerations

### Security
- ✅ DEBUG set to False
- ✅ SECRET_KEY using environment variable
- ✅ Database credentials in environment variables
- ✅ No hardcoded sensitive data

### Performance
- ✅ Gunicorn WSGI server for production
- ✅ Database connection pooling configured
- ✅ Proper error handling and logging

### Monitoring
- Use `/api/v1/health` endpoint for health checks
- Monitor logs in Render dashboard
- Set up alerts for downtime

## 🔍 Troubleshooting

### Common Issues:

1. **Database Connection Failed:**
   - Verify environment variables are set correctly
   - Check if database is running and accessible
   - Ensure firewall allows connections

2. **Module Import Errors:**
   - Check all dependencies are in requirements.txt
   - Verify Python version compatibility

3. **Port Binding Issues:**
   - Render automatically provides PORT environment variable
   - App is configured to use this port

### Debug Commands:
```bash
# Check logs
render logs --service your-service-name

# Test database connection locally
python -c "from database import db; print(db.connect())"

# Test API health
curl https://your-app-name.onrender.com/api/v1/health
```

## 📊 Sample API Calls

### Create a Vendor:
```bash
curl -X POST https://your-app-name.onrender.com/api/v1/vendors \
  -H "Content-Type: application/json" \
  -d '{
    "name": "New Supplier",
    "contact_person": "John Doe",
    "email": "john@supplier.com",
    "phone": "+1-555-0123"
  }'
```

### Get Dashboard Data:
```bash
curl https://your-app-name.onrender.com/api/v1/analytics/dashboard
```

## 🎉 Next Steps

1. **Custom Domain:** Configure custom domain in Render settings
2. **SSL Certificate:** Automatically provided by Render
3. **Scaling:** Upgrade plan for auto-scaling capabilities
4. **Monitoring:** Set up external monitoring tools
5. **Backups:** Configure database backup schedule

---

**🔗 Useful Links:**
- [Render Documentation](https://render.com/docs)
- [Flask Production Deployment](https://flask.palletsprojects.com/en/2.3.x/deploying/)
- [Gunicorn Configuration](https://docs.gunicorn.org/en/stable/configure.html)

**📞 Support:**
- Render Support: [help@render.com](mailto:help@render.com)
- Flask Community: [Flask Discord](https://discord.gg/flask)
