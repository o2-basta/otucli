Good! I can see you have a Supabase project set up (CLI and web components). Here are the main ways to connect to Supabase from your terminal:

## 1. **Using Supabase CLI (Recommended)**

First, install the Supabase CLI:

```bash
npm install -g supabase
```

Then authenticate:

```bash
supabase login
```

You'll be prompted to create an access token on the Supabase dashboard.

## 2. **Connect to Your Project**

In your project directory:

```bash
supabase link --project-id your-project-id
```

You can find your project ID in the Supabase dashboard URL.

## 3. **Access Supabase from Terminal**

Once set up, you can:

```bash
# View database status
supabase status

# Open Supabase dashboard
supabase open

# Start local Supabase (if using local development)
supabase start

# Stop local Supabase
supabase stop
```

## 4. **Direct Database Access via psql**

If you want to connect directly to the PostgreSQL database:

```bash
# Get your connection string from Supabase dashboard (Settings > Database)
psql "postgresql://postgres:password@host:5432/postgres"
```

## 5. **Using Node.js Client (For Your CLI App)**

In your `otu.ai.cli` project, you're already likely using the Supabase client. Check your setup:

```bash
cd otu.ai.cli
npm install @supabase/supabase-js
```

Then connect in your code:

```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
)
```

## 6. **Environment Variables**

Make sure your `.env.local` file has:

```bash
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key  # (optional)
```

Based on your CLAUDE.md file, your project already has this setup templated. Do you need help with any specific Supabase connection task, like querying the database or setting up migrations?
