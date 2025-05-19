{{ ... }}
   node app.js
   ```

4. **Access the application**
   - Website: https://localhost:3002
   - CDN Controller API: https://localhost:3001

## API Endpoints

### CDN Controller API
- `POST /upload` - Upload a file to the CDN
- `GET /files` - List all available files
- `GET /files/:filename` - Download a specific file
- `GET /reset` - Reset the CDN (for testing)

### Website API
- `GET /videos` - List all available videos
- `POST /upload` - Upload a new video

## Configuration

### CDN Controller Configuration (`cdncontroller/config/config.js`)

```javascript
module.exports = {
    db: 'cdn',
    port: 3001,
    edgeServerLoc: [
        { host: 'localhost', port: 4001, bucket: 'bucket1' },
        { host: 'localhost', port: 4002, bucket: 'bucket2' },
        { host: 'localhost', port: 4003, bucket: 'bucket3' }
    ]
};
```

## Deployment

For production deployment, consider the following:

1. Use a process manager like PM2:
   ```bash
   npm install -g pm2
   pm2 start app.js --name "cdn-controller"
   ```

2. Set up Nginx as a reverse proxy
3. Configure proper SSL certificates using Let's Encrypt
4. Set up monitoring and logging

## Security Considerations

- Always use HTTPS in production
- Implement proper authentication and authorization
- Validate and sanitize all user inputs
- Set appropriate CORS headers
- Keep dependencies updated

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Support

For support, please open an issue in the GitHub repository or contact the maintainers.

# 🌐 CDN (Content Delivery Network) Implementation

[![Node.js](https://img.shields.io/badge/Node.js-14%2B-brightgreen)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-4.4%2B-brightgreen)](https://www.mongodb.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A high-performance Content Delivery Network (CDN) implementation with a central controller and multiple edge servers for efficient content distribution. This project demonstrates core CDN concepts including content caching, load balancing, and geographic distribution.

## ✨ Features

### 🎯 CDN Controller
- Centralized content management and distribution
- Intelligent request routing to nearest edge server
- Real-time health monitoring of edge servers
- Automated content replication and invalidation
- Secure file upload and management

### 🌍 Edge Servers
- Distributed content caching
- Low-latency content delivery
- Automatic content synchronization
- Load balancing and failover support
- Bandwidth optimization

### 🌐 Web Interface
- Intuitive dashboard for content management
- Real-time analytics and monitoring
- Responsive design for all devices
- Secure authentication and authorization
- Content upload and management

## 🚀 Quick Start

### Prerequisites

- Node.js 14.x or higher
- MongoDB 4.4 or higher
- OpenSSL (for HTTPS certificates)
- npm or yarn package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/HarshithMuramalla/cdn-implementation.git
   cd cdn-implementation
   ```

2. **Install dependencies**
   ```bash
   # Install controller dependencies
   cd cdncontroller
   npm install
   
   # Install website dependencies
   cd ../website
   npm install
   ```

3. **Set up SSL certificates**
   ```bash
   # Generate self-signed certificates (for development)
   cd cdncontroller
   openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt -days 365 -nodes
   
   # Copy certificates to website directory
   cp server.* ../website/
   ```

4. **Configure the application**
   Update the configuration in `cdncontroller/config/config.js`:
   ```javascript
   module.exports = {
     port: 3000,
     mongoURI: 'mongodb://localhost:27017/cdn',
     edgeServers: [
       { id: 'edge1', url: 'https://edge1.example.com' },
       { id: 'edge2', url: 'https://edge2.example.com' },
       { id: 'edge3', url: 'https://edge3.example.com' }
     ],
     jwtSecret: 'your_jwt_secret_key',
     uploadDir: 'uploads/'
   };
   ```

## 🏃‍♂️ Running the Application

### Start MongoDB
```bash
mongod --dbpath /path/to/data/db
```

### Start CDN Controller
```bash
cd cdncontroller
node app.js
```

### Start Website
```bash
cd website
node app.js
```

### Start Edge Servers
```bash
# In separate terminal windows
node edgeservers/edge1.js
node edgeservers/edge2.js
node edgeservers/edge3.js
```

## 🏗️ Project Structure

```
cdn-implementation/
├── cdncontroller/            # Controller server
│   ├── config/              # Configuration files
│   ├── controller/          # Request handlers
│   ├── model/               # Database models
│   ├── routes/              # API routes
│   ├── uploads/             # Uploaded files
│   ├── app.js               # Main application
│   └── package.json         # Dependencies
│
├── edgeservers/            # Edge server instances
│   ├── edge1.js             # Edge server 1
│   ├── edge2.js             # Edge server 2
│   └── edge3.js             # Edge server 3
│
└── website/                 # Frontend website
    ├── public/              # Static files
    │   ├── css/             # Stylesheets
    │   ├── js/              # JavaScript
    │   ├── images/          # Image assets
    │   └── videos/          # Video content
    ├── views/               # Template files
    ├── app.js               # Website server
    └── package.json         # Dependencies
```

## 🔧 Configuration

### Environment Variables

Create a `.env` file in both `cdncontroller/` and `website/` directories:

```env
NODE_ENV=development
PORT=3000
MONGODB_URI=mongodb://localhost:27017/cdn
JWT_SECRET=your_jwt_secret
UPLOAD_DIR=uploads/
```

### Edge Server Configuration

Each edge server can be configured with the following environment variables:

```env
EDGE_ID=edge1
CONTROLLER_URL=https://controller.example.com
PORT=4000
CACHE_DIR=./cache
CACHE_TTL=3600
```

## 📡 API Endpoints

### Controller API

| Method | Endpoint           | Description                     |
|--------|-------------------|---------------------------------|
| GET    | /api/health       | Check controller health        |
| POST   | /api/upload       | Upload file to CDN             |
| GET    | /api/files        | List all files                 |
| GET    | /api/files/:id    | Get file details               |
| DELETE | /api/files/:id    | Delete file                    |
| GET    | /api/edges        | List all edge servers          |
| GET    | /api/edges/:id    | Get edge server details        |

### Edge Server API

| Method | Endpoint           | Description                     |
|--------|-------------------|---------------------------------|
| GET    | /health           | Check edge server health       |
| GET    | /:fileId          | Get file from edge server      |
| POST   | /sync             | Sync file from controller      |
| DELETE | /:fileId          | Remove file from cache         |

## 📊 Monitoring

The CDN includes built-in monitoring capabilities:

1. **Real-time Dashboard**
   - View system status
   - Monitor edge server health
   - Track request metrics

2. **Logging**
   - Request logging
   - Error tracking
   - Performance metrics

3. **Alerts**
   - Server health alerts
   - Performance degradation alerts
   - Security incident alerts

## 🔒 Security

- **HTTPS** - All communications are encrypted
- **Authentication** - JWT-based authentication
- **Rate Limiting** - Protection against DDoS attacks
- **CORS** - Cross-Origin Resource Sharing configuration
- **Input Validation** - Protection against injection attacks

## 📦 Deployment

### Docker

```bash
# Build and start all services
docker-compose up --build

# Start specific service
docker-compose up cdn-controller
```

### Kubernetes

```bash
# Apply Kubernetes configurations
kubectl apply -f k8s/

# Check pod status
kubectl get pods
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Node.js](https://nodejs.org/)
- [Express](https://expressjs.com/)
- [MongoDB](https://www.mongodb.com/)
- [Bootstrap](https://getbootstrap.com/)
- [Font Awesome](https://fontawesome.com/)

## 📞 Contact

Harshith Muramalla - [@your_twitter](https://twitter.com/your_twitter) - your.email@example.com

Project Link: [https://github.com/HarshithMuramalla/cdn-implementation](https://github.com/HarshithMuramalla/cdn-implementation)
