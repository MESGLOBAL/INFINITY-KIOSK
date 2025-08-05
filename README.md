# Infinity Kiosk - Restaurant Ordering System

A modern, offline-ready restaurant kiosk system built with React, Express.js, and MongoDB. Features QR code table ordering, real-time order management, and progressive web app capabilities.

## 🌟 Features

- **Multi-Module Architecture**: Customer client, kitchen display, and backend server
- **Offline-Ready**: Service workers with IndexedDB queue for disconnected operation
- **QR Code Ordering**: Table-based ordering system with unique QR codes
- **Real-time Updates**: Live order status tracking and kitchen display
- **Progressive Web App**: Installable on mobile devices with app-like experience
- **Responsive Design**: Tailwind CSS for mobile-first responsive interface
- **Secure Payments**: Stripe Checkout integration with PCI SAQ-A compliance
- **Offline Payment Handling**: Queue orders when offline, process when connection returns
- **Cost-Effective**: Free in test mode, 2.9% + 30¢ in production

## 💰 **Stripe Costs & Security**

### **Development (Current)**
- ✅ **FREE FOREVER** while building and testing
- ✅ All transactions are test mode → **$0 cost**
- ✅ No real money processed

### **Production**
- 💳 **2.9% + 30¢** per successful transaction
- 🔒 **PCI SAQ-A / Level 4** compliance (lowest level)
- 🛡️ **No card data** on your servers
- 📋 See `STRIPE_SECURITY_AND_COSTS.md` for complete details

### **Security Keys**
- ⚠️ **Never commit `.env` to Git** - Add `.env` to `.gitignore`
- 🔐 All API keys stored in environment variables
- ✅ Webhook signature verification implemented

## 🏗️ Project Structure

```
infinity-kiosk/
├── client/          # Customer-facing React app
├── server/          # Express.js API server
├── kitchen/         # Kitchen display React app
└── README.md        # This file
```

## 🚀 Quick Start

### Prerequisites

- Node.js (v16 or higher)
- MongoDB (local installation or MongoDB Atlas)
- Git

### 1. Clone and Setup

```bash
# Clone the repository
cd "C:\Users\MES GLOBAL\OneDrive\Desktop\INFINITY KIOSK\MESGLOBAL-AI-AUTOMATION"
cd infinity-kiosk

# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client  
npm install

# Install kitchen dependencies
cd ../kitchen
npm install
```

### 2. Environment Configuration

Create a `.env` file in the `server` directory:

```env
MONGO_URI=mongodb://localhost:27017/infinity-kiosk
PORT=5000
NODE_ENV=development
```

For MongoDB Atlas (cloud database):
```env
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/infinity-kiosk
PORT=5000
NODE_ENV=production
```

### 3. Database Setup

```bash
# Start MongoDB (if using local installation)
mongod

# Seed the database with sample data
cd server
node database/seed.js
```

### 4. Start the Applications

**Terminal 1 - Server:**
```bash
cd server
npm start
```

**Terminal 2 - Client:**
```bash
cd client
npm start
```

**Terminal 3 - Kitchen Display:**
```bash
cd kitchen
npm start
```

The applications will be available at:
- Customer Client: http://localhost:3000
- Kitchen Display: http://localhost:3001  
- API Server: http://localhost:5000

## 📱 Progressive Web App Setup

### Service Worker Registration

The service worker is automatically registered when the client app starts. To test PWA features:

1. Open Chrome DevTools → Application → Service Workers
2. Check "Offline" to simulate network disconnection
3. Verify that cached resources still load
4. Test offline order queueing functionality

### Installation

1. Visit the client app in Chrome/Edge
2. Look for the "Install" button in the address bar
3. Click to install as a standalone app
4. Access from desktop/home screen

## 🔧 API Endpoints

### Menu Endpoints
- `GET /api/menu` - Retrieve all menu items

### Order Endpoints  
- `POST /api/order` - Create a new order
- `PATCH /api/order/:id/status` - Update order status
- `GET /api/orders/live` - Get real-time order updates

### Example Order Payload
```json
{
  "storeId": "store_001",
  "tableId": "table_005", 
  "items": [
    {
      "menuItemId": "64a1b2c3d4e5f6789012345",
      "name": "Classic Burger",
      "price": 12.99,
      "quantity": 2
    }
  ],
  "total": 25.98
}
```

## 🔄 Offline Functionality

### How It Works

1. **Service Worker Caching**: Static assets and API responses are cached
2. **IndexedDB Queue**: Failed requests are stored locally
3. **Background Sync**: Automatic retry when connection restored
4. **Network Detection**: Real-time online/offline status

### Testing Offline Features

```javascript
// In browser console
infinityKioskTests.runAllTests()
```

### Manual Testing Steps

1. Load the application while online
2. Disable network in DevTools (Network tab → Offline)
3. Try placing an order (should queue locally)
4. Re-enable network
5. Orders should automatically sync

## 🛠️ Development

### Available Scripts

**Server:**
- `npm start` - Start production server
- `npm run dev` - Start development server with nodemon
- `npm run seed` - Populate database with sample data

**Client/Kitchen:**
- `npm start` - Start development server
- `npm run build` - Build for production
- `npm test` - Run test suite

### Database Models

**MenuItem Schema:**
```javascript
{
  name: String,
  price: Number,
  category: String,
  photo: String,
  inStock: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

**Order Schema:**
```javascript
{
  storeId: String,
  tableId: String,
  items: [OrderItemSchema],
  total: Number,
  status: ['pending', 'preparing', 'ready', 'completed'],
  createdAt: Date,
  updatedAt: Date
}
```

## 🎯 QR Code Integration

### Generating Table QR Codes

```javascript
import { generateTableQR } from './utils/qrUtils';

const qrCodeUrl = generateTableQR('store_001', 'table_005');
// Returns: http://localhost:3000/order?store=store_001&table=table_005
```

### QR Code Component Usage

```jsx
import TableQRCode from './components/TableQRCode';

<TableQRCode 
  storeId="store_001"
  tableId="table_005"
  size={200}
/>
```

## 🚀 Deployment

### Production Build

```bash
# Build client
cd client
npm run build

# Build kitchen app
cd ../kitchen  
npm run build

# Server is ready for production
cd ../server
NODE_ENV=production npm start
```

### Environment Variables (Production)

```env
MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/infinity-kiosk
PORT=5000
NODE_ENV=production
CORS_ORIGIN=https://yourdomain.com
```

## 🧪 Testing

### Unit Tests
```bash
npm test
```

### Integration Tests
```bash
# Test API endpoints
npm run test:api

# Test offline functionality  
npm run test:offline
```

### Manual Testing Checklist

- [ ] Place order while online
- [ ] Place order while offline
- [ ] Verify order sync when reconnected
- [ ] Test QR code generation
- [ ] Verify kitchen display updates
- [ ] Test PWA installation
- [ ] Check service worker caching

## 🐛 Troubleshooting

### Common Issues

**MongoDB Connection Failed:**
```bash
# Check if MongoDB is running
mongod --version

# Start MongoDB service
brew services start mongodb/brew/mongodb-community  # macOS
sudo systemctl start mongod                          # Linux
```

**Service Worker Not Registering:**
- Ensure HTTPS or localhost
- Check browser console for errors
- Verify service worker file exists at `/public/sw.js`

**Orders Not Syncing:**
- Check network connectivity
- Verify API server is running
- Check browser IndexedDB in DevTools

**QR Codes Not Generating:**
- Ensure `qrcode.react` is installed
- Check React component props
- Verify store/table IDs are valid

## 📊 Performance

### Optimization Features

- Service worker caching strategy
- Lazy loading of components
- Optimized bundle sizes with code splitting
- IndexedDB for efficient local storage
- Background sync for seamless UX

### Monitoring

Monitor application performance:
- Check Network tab for API response times
- Use Lighthouse for PWA audit
- Monitor IndexedDB usage in Application tab
- Track service worker cache hit rates

## 🔒 **Security & Compliance**

### **Payment Security**
- ✅ **PCI SAQ-A / Level 4** compliance (lowest burden)
- ✅ **No card data** stored on servers
- ✅ **Stripe Checkout** handles all sensitive information
- ✅ **Webhook signature verification** implemented
- 📋 See `STRIPE_SECURITY_AND_COSTS.md` for complete details

### **Environment Security**
```bash
# ⚠️ CRITICAL: Verify .env protection
git status --ignored | grep .env
# Should show .env files are ignored

# Quick security setup
cp server/.env.example server/.env
cp client/.env.example client/.env
# Edit with your Stripe TEST keys (FREE)
```

### **API Key Safety**
- 🔐 **Test keys** (sk_test_, pk_test_) are **FREE and safe**
- ⚠️ **Live keys** (sk_live_, pk_live_) process **real money**
- 📋 Follow `SECURITY_SETUP.md` for step-by-step configuration

### **Cost Transparency**
```
Development: FREE (test mode)
Production: 2.9% + 30¢ per transaction
No monthly fees or hidden costs
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🔗 Links

- [React Documentation](https://reactjs.org/)
- [Express.js Guide](https://expressjs.com/)
- [MongoDB Manual](https://docs.mongodb.com/)
- [Service Workers API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [PWA Documentation](https://web.dev/progressive-web-apps/)

---

**Built with ❤️ for the restaurant industry**
