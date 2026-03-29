# Mela Pay

**Learn Smarter, Pay with Crypto**

Mela Pay is a blockchain-powered education platform that enables users to purchase EdX courses using Polkadot (DOT) cryptocurrency. The platform bridges traditional online education with decentralized finance, making high-quality learning accessible to the crypto community.

---

## What Problem Does It Solve?

Traditional online course platforms primarily accept fiat payments (credit cards, bank transfers), which creates barriers for:

- **Crypto holders** who prefer spending DOT rather than converting to fiat
- **Unbanked populations** who have access to crypto wallets but not traditional banking
- **Global learners** facing currency conversion fees and international payment restrictions
- **Privacy-conscious users** who prefer pseudonymous blockchain transactions

Mela Pay solves these issues by allowing direct DOT payments for EdX courses, eliminating intermediaries and providing a seamless crypto-to-education experience.

---

## Technology Stack

### Backend
| Technology | Purpose |
|------------|---------|
| **Node.js** | Runtime environment |
| **Express.js** | Web framework |
| **MongoDB** | Database for courses, payments, and users |
| **Mongoose** | ODM for MongoDB |
| **JWT** | Authentication |
| **NowPayments API** | Crypto payment processing |
| **EdX Catalog API** | Course data source |
| **Nodemailer** | Email notifications |
| **Passport.js** | OAuth (Google authentication) |

### Frontend
| Technology | Purpose |
|------------|---------|
| **Next.js 13** | React framework |
| **React 18** | UI library |
| **Tailwind CSS** | Styling |
| **React Query** | Data fetching and caching |
| **Polkadot JS API** | Blockchain integration |
| **Polkadot JS Extension** | Wallet connection |
| **QRCode.react** | Payment QR codes |
| **Axios** | HTTP client |

---

## How It Works

### User Flow

```
1. Browse Courses → View 1000+ EdX courses with search and filtering
        ↓
2. Add to Cart → Select multiple courses and view cart
        ↓
3. Checkout → Enter email and name for purchase
        ↓
4. Connect Wallet → Link Polkadot wallet via browser extension
        ↓
5. Generate Payment → System creates unique DOT payment address
        ↓
6. Send DOT → User transfers exact amount (via QR code or manual)
        ↓
7. Confirmation → Blockchain detects payment automatically
        ↓
8. Course Access → User receives enrollment confirmation via email
```

### Architecture Overview

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend      │────▶│   Backend API   │────▶│   MongoDB       │
│   (Next.js)     │     │   (Express)     │     │   (Database)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │
        ▼                       ▼
┌─────────────────┐     ┌─────────────────┐
│   Polkadot      │     │   NowPayments   │
│   Extension     │     │   API           │
└─────────────────┘     └─────────────────┘
                                │
                                ▼
                        ┌─────────────────┐
                        │   EdX Courses   │
                        │   API           │
                        └─────────────────┘
```

### Payment States

| State | Description |
|-------|-------------|
| `pending` | Payment created, awaiting user action |
| `waiting` | Payment address generated, waiting for DOT transfer |
| `confirming` | Payment detected on blockchain, confirming |
| `confirmed` | Payment confirmed with sufficient confirmations |
| `finished` | Payment complete, course access granted |
| `failed` | Payment failed or insufficient amount |
| `expired` | Payment timeout (typically 24 hours) |

---

## Getting Started

### Prerequisites

Before using Mela Pay, you **must** install the Polkadot JS Extension:

**[Install Polkadot{js} Extension →](https://chromewebstore.google.com/detail/polkadot%7Bjs%7D-extension/mopnmbcafieddcagagdcbnhejhlodfdd)**

After installation:
1. Create or import a Polkadot wallet
2. Fund your wallet with DOT tokens
3. Keep the extension enabled in your browser

---

### Local Development Setup

#### 1. Clone and Navigate

```bash
cd mela-chain
```

#### 2. Backend Setup

```bash
cd backend
npm install

# Create environment file
cp .env.example .env

# Edit .env with your credentials:
# - MongoDB URI
# - NowPayments API key
# - JWT secret
# - Email SMTP settings

# Start development server
npm run dev
```

Backend will run on `http://localhost:5000`

#### 3. Frontend Setup

```bash
cd frontend
npm install

# Create environment file
cp .env.local.example .env.local

# Edit .env.local:
# NEXT_PUBLIC_API_URL=http://localhost:5000/api

# Start development server
npm run dev
```

Frontend will run on `http://localhost:3000`

#### 4. Seed Database (Optional)

```bash
cd backend
npm run seed
```

This populates the database with sample EdX courses.

---

### Environment Variables

#### Backend (.env)

```env
# MongoDB
MONGODB_URI=mongodb://localhost:27017/mela-chain

# Server
PORT=5000
NODE_ENV=development

# JWT
JWT_SECRET=your_super_secret_jwt_key

# NowPayments
NOWPAYMENTS_API_KEY=your_nowpayments_api_key
NOWPAYMENTS_API_URL=https://api.nowpayments.io/v1

# Email (Gmail SMTP)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password

# Google OAuth (optional)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
```

#### Frontend (.env.local)

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_APP_NAME=Mela Pay
```

---

## Project Structure

```
mela-chain/
├── backend/
│   ├── controllers/      # Request handlers
│   ├── models/          # Database schemas
│   ├── routes/          # API endpoints
│   ├── services/        # Business logic
│   ├── middleware/      # Auth, error handling
│   └── server.js        # Entry point
│
├── frontend/
│   ├── pages/           # Next.js routes
│   ├── components/      # React components
│   ├── context/         # Cart and auth contexts
│   ├── lib/             # API utilities
│   └── styles/          # CSS and Tailwind
│
└── README.md            # This file
```

---

## Key Features

- **Course Catalog**: Browse 1000+ EdX courses with search, filters, and categories
- **Shopping Cart**: Add multiple courses, persistent cart with localStorage
- **DOT Payments**: Pay with Polkadot cryptocurrency via NowPayments
- **QR Code Payments**: Mobile-friendly payment QR codes
- **Real-time Tracking**: Watch payment status update as blockchain confirms
- **Email Notifications**: Receipts and course access info delivered to inbox
- **User Authentication**: JWT-based auth with optional Google OAuth
- **Admin Dashboard**: Manage courses, view analytics, track payments
- **Responsive Design**: Works on desktop, tablet, and mobile

---

## API Endpoints

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/mela/courses` | List all courses |
| GET | `/api/mela/courses/:id` | Get course details |
| GET | `/api/mela/courses/search?q=query` | Search courses |
| POST | `/api/mela/payments/create` | Create new payment |
| GET | `/api/mela/payments/:id` | Get payment status |

### Admin Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/mela/admin/login` | Admin authentication |
| GET | `/api/mela/admin/dashboard` | Dashboard stats |
| GET | `/api/mela/admin/payments` | List all payments |
| POST | `/api/mela/admin/sync-courses` | Sync with EdX |

---

## Deployment

### Backend Deployment (Railway/Heroku/Render)

1. Push code to GitHub
2. Connect repository to hosting platform
3. Set environment variables
4. Configure webhook URL for NowPayments
5. Deploy

### Frontend Deployment (Vercel/Netlify)

1. Connect GitHub repository
2. Set build command: `npm run build`
3. Set output directory: `dist` (or default)
4. Configure environment variables
5. Deploy

---

## Security Considerations

- JWT tokens expire after 24 hours
- Passwords hashed with bcrypt (10 rounds)
- CORS configured for specific origins
- Webhook signatures verified from NowPayments
- No sensitive data exposed in frontend
- Input validation on all endpoints

---

## Troubleshooting

### Extension Not Detected
- Ensure Polkadot{js} extension is installed and enabled
- Refresh the page after enabling
- Check browser console for errors

### Payment Not Confirming
- Verify sufficient DOT amount sent
- Check transaction on Polkadot explorer
- Ensure payment made within 24-hour window

### API Connection Failed
- Verify backend running on correct port
- Check CORS settings match frontend URL
- Confirm environment variables set correctly

---

## License

MIT License - Free for personal and commercial use

---

## Support

- **GitHub Issues**: Report bugs or request features
- **Documentation**: Check files in `mela-chain/` directory

---

**Built for the future of education**

*Mela Pay - Learn Smarter, Pay with Crypto*
