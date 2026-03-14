# Supabase Setup Guide

Detailed step-by-step guide to set up a Supabase project for use with GenSpark AI Sheets.

## Prerequisites

- Supabase account (free at [supabase.io](https://supabase.io))
- PostgreSQL knowledge (basic)
- Internet connection

## Step-by-Step Setup

### 1. Create Supabase Project

1. Go to [app.supabase.com](https://app.supabase.com)
2. Sign in with your credentials
3. Click "New Project"
4. Fill in:
   - **Name**: `genspark-demo` (or your choice)
   - **Password**: Create a secure database password
   - **Region**: Choose closest to your location
5. Wait for project initialization (2-3 minutes)

### 2. Create Database Table

1. In the Supabase dashboard, go to **SQL Editor**
2. Click **New Query**
3. Paste this SQL:

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE
);
```

4. Click "Run"
5. You should see "Success" message

### 3. Add Sample Data

In the SQL Editor, run:

```sql
INSERT INTO users (name, email) VALUES
  ('Alice Johnson', 'alice@example.com'),
  ('Bob Smith', 'bob@example.com'),
  ('Carol Williams', 'carol@example.com'),
  ('David Brown', 'david@example.com');
```

Or use the Table Editor:

1. Go to **Table Editor**
2. Click on **users** table
3. Click **Insert row**
4. Fill in the fields
5. Click **Save**

### 4. Set Up Row Level Security (RLS)

RLS allows you to control who can access data.

1. Go to **Authentication** → **Policies**
2. Select **users** table
3. Click **New Policy**
4. Choose **For SELECT (read access)**
5. Set policy:
   - **Policy Name**: `Enable read for all`
   - **For**: `All users`
   - **Using expression**: `true`
6. Click **Review** → **Save policy**

**Optional - Enable Insert for testing:**

1. Create another policy for INSERT
2. Set **Using expression**: `true`
3. Set **With check expression**: `true`

### 5. Get API Credentials

1. Go to **Project Settings** (bottom left)
2. Click **API**
3. Copy these values:
   - **Project URL** (under "Project URL")
   - **Publishable Key** (under "Anon public")

Example:
```
Project URL: https://abcdefghijklmnopqrst.supabase.co
Publishable Key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### 6. Test REST API

Open your browser and test the REST endpoint:

```
https://[your-project].supabase.co/rest/v1/users?apikey=[your-publishable-key]
```

You should see JSON response with your data.

## Advanced Configuration

### Enable Updates and Deletes

For production, create specific policies for UPDATE and DELETE:

```sql
-- Policy for UPDATE
CREATE POLICY "Enable update for all users" ON users
FOR UPDATE USING (true) WITH CHECK (true);

-- Policy for DELETE
CREATE POLICY "Enable delete for all users" ON users
FOR DELETE USING (true);
```

### Add User Authentication

1. Go to **Authentication** → **Users**
2. Click **Invite** to add users
3. Create specific RLS policies per user:

```sql
CREATE POLICY "Users can read own data" ON users
FOR SELECT USING (auth.uid() = id);
```

### Monitor Database

1. Go to **Database** → **Usage**
2. Monitor:
   - Database size
   - API requests
   - Real-time connections

### Backup Data

1. Go to **Project Settings** → **Backups**
2. Enable automated backups
3. Download manual backups as needed

## Common Issues and Solutions

### Issue: "CORS Error" in Browser

**Solution**:
1. Go to **Project Settings** → **API**
2. Scroll to "CORS settings"
3. Add your GenSpark domain
4. Or set to `*` for testing (not recommended for production)

### Issue: "Unauthorized" Error

**Solution**:
- Check API key is correct
- Verify RLS policy allows access
- Check policy expression is `true` for testing

### Issue: "Table does not exist"

**Solution**:
- Verify table name matches exactly (case-sensitive)
- Check in Table Editor that table was created
- Run SQL query in SQL Editor to verify

### Issue: "Row limit exceeded"

**Solution**:
- Use pagination with `.limit()` and `.offset()` parameters
- Example: `?limit=100&offset=0`

## Performance Optimization

### Add Indexes

For frequently searched columns:

```sql
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at);
```

### Pagination Query

```
GET /rest/v1/users?limit=10&offset=0
```

### Filtering Query

```
GET /rest/v1/users?email=eq.alice@example.com
```

## Security Best Practices

✅ Always use RLS policies
✅ Never expose secret keys in client-side code
✅ Use environment variables for sensitive data
✅ Rotate API keys regularly
✅ Use HTTPS for all requests
✅ Implement rate limiting
✅ Monitor usage and logs
✅ Keep Supabase updated

## Next Steps

1. Verify table and data are created
2. Test REST API endpoint in browser
3. Proceed to GenSpark AI Sheets configuration
4. Use GENSPARK_CONFIG.md for integration steps

## Additional Resources

- [Supabase Docs](https://supabase.io/docs)
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/)
- [REST API Guide](https://supabase.io/docs/reference/api)
- [RLS Guide](https://supabase.io/docs/learn/auth-deep-dive/row-level-security)

## Support

For issues:
1. Check [Supabase Status](https://status.supabase.com)
2. Visit [Supabase Discord](https://discord.supabase.com)
3. Open issue on [Supabase GitHub](https://github.com/supabase/supabase)

---

**Last Updated**: March 14, 2026
**Supabase Version**: Latest
**Status**: ✅ Tested and Working
