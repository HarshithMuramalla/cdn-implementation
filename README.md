# CDN (Content Delivery Network) Implementation

This project implements a simple Content Delivery Network (CDN) with a controller server and edge servers for efficient content distribution. The system is built using Node.js, Express, MongoDB, and HTTP/2 for improved performance.

## Project Structure

```
cdn-controller/        # Main controller server
├── config/            # Configuration files
├── controller/        # Request handlers
├── model/             # Database models
├── routes/            # API routes
├── uploads/           # Uploaded files
├── app.js             # Main application entry
└── package.json       # Dependencies

edge-servers/          # Edge server instances
├── bucket1/           # Edge server 1 storage
├── bucket2/           # Edge server 2 storage
└── bucket3/           # Edge server 3 storage

website/               # Frontend website
├── public/            # Static files
│   ├── css/           # Stylesheets
│   ├── js/            # JavaScript files
│   ├── images/        # Image assets
│   └── videos/        # Video content
├── app.js             # Website server
└── package.json       # Dependencies
```

## Features

1. **CDN Controller**
   - Manages content distribution to edge servers
   - Handles file uploads and distribution
   - Monitors edge server health
   - Implements content replication

2. **Edge Servers**
   - Cache and serve content to users
   - Automatically sync content from controller
   - Distribute load across multiple servers

3. **Website**
   - User interface for uploading and viewing content
   - Video streaming capabilities
   - Responsive design for all devices

## Prerequisites

- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- npm (Node Package Manager)
- OpenSSL (for HTTPS certificates)

## Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd cdn
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
   - Place your SSL certificates (`server.crt` and `server.key`) in both `cdncontroller/` and `website/` directories
   - Or generate self-signed certificates using OpenSSL:
     ```bash
     openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt -days 365 -nodes
     ```

4. **Configure the application**
   - Update the configuration in `cdncontroller/config/config.js` with your MongoDB connection string and server settings

## Running the Application

1. **Start MongoDB**
   ```bash
   mongod
   ```

2. **Start the CDN Controller**
   ```bash
   cd cdncontroller
   node app.js
   ```

3. **Start the Website**
   ```bash
   cd website
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
