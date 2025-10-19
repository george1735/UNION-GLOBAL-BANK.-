union-global-bank/
├── frontend/
│   ├── public/
│   │   ├── index.html
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.js
│   │   │   ├── Dashboard.js
│   │   │   ├── Login.js
│   │   │   ├── Register.js
│   │   │   └── AdminPanel.js
│   │   ├── App.js
│   │   ├── index.js
│   │   └── styles.css
│   ├── package.json
├── backend/
│   ├── config/
│   │   ├── database.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── transactions.js
│   │   └── admin.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── transactionController.js
│   │   └── adminController.js
│   ├── middleware/
│   │   ├── auth.js
│   ├── server.js
│   ├── .env
│   ├── package.json
├── database/
│   ├── schema.sql
├── .gitignore
├── README.md
# Union Global Bank - Advanced Online Banking Platform

This repository contains the source code for an advanced online banking platform built for "Union Global Bank". The platform includes a React frontend, Node.js/Express backend, and a PostgreSQL database integrated with Supabase. It features user authentication, a user dashboard, transaction management, and an advanced admin panel.

## Features
- User authentication (registration, login, password reset) with Supabase Auth
- Dashboard with account overview and transaction history
- Fund transfer functionality
- Advanced admin panel with user and transaction management
- Multi-currency support, card management, loans, and fixed deposits
- Custom transactional email notifications
- Real-time fraud detection and audit logs

## Tech Stack
- **Frontend:** React.js
- **Backend:** Node.js/Express
- **Database:** PostgreSQL (Supabase)
- **Authentication:** Supabase Auth
- **Email:** SendGrid integration

## Installation
1. Clone the repository: `git clone https://github.com/your-username/union-global-bank.git`
2. Navigate to the project directory: `cd union-global-bank`
3. Install frontend dependencies: `cd frontend && npm install`
4. Install backend dependencies: `cd ../backend && npm install`
5. Set up Supabase and configure `.env` with your Supabase URL, key, and SendGrid API key.
6. Run the backend: `npm start` in the `backend` directory.
7. Run the frontend: `npm start` in the `frontend` directory.

## Database Setup
- Import `database/schema.sql` into your Supabase project.
- Configure Row Level Security (RLS) policies as outlined in the schema.

## Contributing
Feel free to fork this repository and submit pull requests for improvements.

## License
MIT License
node_modules/
dist/
.env
{
  "name": "union-global-bank-frontend",
  "version": "1.0.0",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.4.0",
    "@supabase/supabase-js": "^2.0.0",
    "axios": "^1.0.0"
  },
  "devDependencies": {
    "react-scripts": "^5.0.0"
  }
}
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Navbar from './components/Navbar';
import Login from './components/Login';
import Register from './components/Register';
import Dashboard from './components/Dashboard';
import AdminPanel from './components/AdminPanel';
import './styles.css';

function App() {
  return (
    <Router>
      <div className="app">
        <Navbar />
        <Routes>
          <Route path="/" element={<Login />} />
          <Route path="/register" element={<Register />} />
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/admin" element={<AdminPanel />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
import React from 'react';

const Navbar = () => {
  return (
    <nav className="navbar">
      <div className="logo">Union Global Bank</div>
      <ul className="nav-links">
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/faq">FAQ</a></li>
        <li><a href="/contact">Contact</a></li>
        <li><a href="/login" className="sign-in">Sign In</a></li>
      </ul>
      <div className="support">
        <span>24/7 Support</span>
        <a href="https://wa.me/1234567890" target="_blank"><i className="fab fa-whatsapp"></i></a>
        <a href="/chat"><i className="fas fa-comment"></i></a>
      </div>
    </nav>
  );
};

export default Navbar;
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f0f4f8;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #003087;
  color: white;
  padding: 1rem;
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 1rem;
}

.nav-links a {
  color: white;
  text-decoration: none;
}

.sign-in {
  background-color: #ffcc00;
  padding: 0.5rem 1rem;
  border-radius: 5px;
}

.support {
  display: flex;
  gap: 1rem;
}

.support a {
  color: white;
  font-size: 1.2rem;
}
{
  "name": "union-global-bank-backend",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "supabase": "^1.0.0",
    "@sendgrid/mail": "^7.7.0",
    "dotenv": "^16.0.0",
    "jsonwebtoken": "^9.0.0"
  }
}
require('dotenv').config();
const express = require('express');
const authRoutes = require('./routes/auth');
const transactionRoutes = require('./routes/transactions');
const adminRoutes = require('./routes/admin');
const { createClient } = require('@supabase/supabase-js');

const app = express();
const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY);

app.use(express.json());
app.use('/auth', authRoutes);
app.use('/transactions', transactionRoutes);
app.use('/admin', adminRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
const express = require('express');
const router = express.Router();
const authController = require('../controllers/authController');

router.post('/register', authController.register);
router.post('/login', authController.login);
router.post('/reset-password', authController.resetPassword);

module.exports = router;
-- Enable extensions
create extension if not exists "uuid-ossp";
create extension if not exists pgcrypto;

-- User Profiles
create table public.user_profiles (
  id uuid primary key default gen_random_uuid(),
  auth_uid uuid references auth.users(id) on delete cascade,
  full_name text not null,
  email text not null,
  phone text,
  created_at timestamptz default now(),
  updated_at timestamptz default now(),
  kyc_status text default 'pending',
  kyc_documents jsonb,
  metadata jsonb
);

-- Bank Accounts
create table public.bank_accounts (
  id uuid primary key default gen_random_uuid(),
  user_profile_id uuid references public.user_profiles(id) on delete cascade,
  account_number text unique not null,
  account_type text not null,
  currency char(3) not null default 'USD',
  created_at timestamptz default now(),
  status text default 'active',
  product jsonb,
  metadata jsonb
);

-- Account Balances
create table public.account_balances (
  account_id uuid primary key references public.bank_accounts(id) on delete cascade,
  balance numeric(18,2) default 0,
  pending_balance numeric(18,2) default 0,
  available_balance numeric(18,2) default 0,
  updated_at timestamptz default now()
);

-- Ledger Entries
create table public.ledger_entries (
  id uuid primary key default gen_random_uuid(),
  tx_id uuid not null,
  account_id uuid references public.bank_accounts(id) on delete cascade,
  entry_type text not null,
  amount numeric(18,2) not null check (amount >= 0),
  currency char(3) not null,
  description text,
  metadata jsonb,
  created_by uuid references public.user_profiles(id),
  created_at timestamptz default now()
);

create index on public.ledger_entries (tx_id);
create index on public.ledger_entries (account_id);

-- Transactions
create table public.transactions (
  id uuid primary key default gen_random_uuid(),
  tx_reference text unique not null,
  initiator_profile_id uuid references public.user_profiles(id),
  from_account uuid references public.bank_accounts(id),
  to_account uuid references public.bank_accounts(id),
  amount numeric(18,2) not null,
  currency char(3) not null default 'USD',
  status text default 'pending',
  initiated_at timestamptz default now(),
  completed_at timestamptz,
  failure_reason text,
  metadata jsonb
);

-- Transaction Function
create or replace function public.create_transaction(
  p_tx_reference text,
  p_initiator uuid,
  p_from_account uuid,
  p_to_account uuid,
  p_amount numeric,
  p_currency char(3),
  p_description text
) returns uuid as $$
declare
  v_tx_id uuid := gen_random_uuid();
begin
  if p_amount <= 0 then
    raise exception 'amount must be > 0';
  end if;
  insert into public.transactions(id, tx_reference, initiator_profile_id, from_account, to_account, amount, currency, status, initiated_at)
  values (v_tx_id, p_tx_reference, p_initiator, p_from_account, p_to_account, p_amount, p_currency, 'pending', now());
  insert into public.ledger_entries(tx_id, account_id, entry_type, amount, currency, description, created_by)
  values (v_tx_id, p_from_account, 'debit', p_amount, p_currency, p_description, p_initiator);
  insert into public.ledger_entries(tx_id, account_id, entry_type, amount, currency, description, created_by)
  values (v_tx_id, p_to_account, 'credit', p_amount, p_currency, p_description, p_initiator);
  update public.account_balances set balance = balance - p_amount, updated_at = now() where account_id = p_from_account;
  update public.account_balances set balance = balance + p_amount, updated_at = now() where account_id = p_to_account;
  update public.transactions set status = 'completed', completed_at = now() where id = v_tx_id;
  return v_tx_id;
exception
  when others then
    update public.transactions set status = 'failed', failure_reason = sqlerrm where tx_reference = p_tx_reference;
    raise;
end;
$$ language plpgsql security definer;

-- RLS Policies
alter table public.user_profiles enable row level security;
create policy "Profiles: owner can view" on public.user_profiles for select using (auth.uid()::text = auth_uid::text);
create policy "Profiles: owner can update" on public.user_profiles for update using (auth.uid()::text = auth_uid::text);

alter table public.bank_accounts enable row level security;
create policy "Accounts: owner access" on public.bank_accounts for all using (exists (
  select 1 from public.user_profiles up where up.id = user_profile_id and up.auth_uid::text = auth.uid()::text
));
SUPABASE_URL=your_supabase_url
SUPABASE_KEY=your_supabase_key
SENDGRID_API_KEY=your_sendgrid_api_key
PORT=5000
