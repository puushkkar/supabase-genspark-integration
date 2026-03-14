# supabase-genspark-integration
Integration of Supabase REST API with GenSpark AI Sheets. This project demonstrates how to connect a Supabase database to GenSpark AI Sheets for real-time data visualization and manipulation.

## Project Overview

This project demonstrates a complete integration between Supabase (a Firebase alternative with PostgreSQL) and GenSpark AI Sheets. It shows how to:

- Create a Supabase database project
- Set up authentication and RLS (Row Level Security) policies
- Connect to Supabase REST API from GenSpark AI Sheets
- Manage data through both platforms

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                  Supabase Backend                           │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ PostgreSQL Database (users table)                    │  │
│  │ - id (UUID, PK)                                      │  │
│  │ - name (text)                                        │  │
│  │ - email (text)                                       │  │
│  │ - created_at (timestamp)                             │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ REST API (Auto-generated)                            │  │
│  │ Endpoint: /rest/v1/users                             │  │
│  │ Authentication: API Key (Publishable)                │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↓↑
                      REST API Call
                    (GET, POST, PUT, DELETE)
                           ↓↑
┌─────────────────────────────────────────────────────────────┐
│             GenSpark AI Sheets Frontend                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ DataSource Configuration                            │  │
│  │ - Type: REST API                                     │  │
│  │ - URL: https://[project].supabase.co/rest/v1/users  │  │
│  │ - Auth: API Key (Query Parameter)                   │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Spreadsheet with live data binding                  │  │
│  │ - Columns auto-mapped from API response             │  │
│  │ - Real-time data manipulation                        │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Setup Instructions

### Step 1: Supabase Setup

1. Go to [supabase.io](https://supabase.io) and sign up/login
2. Create a new project
3. Create a `users` table with the following schema:
   ```sql
   CREATE TABLE users (
     id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
     created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
     name TEXT,
     email TEXT
   );
   ```
4. Insert sample data:
   ```sql
   INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
   ```
5. Set up RLS Policy for public read access:
   - Go to Authentication → Policies
   - Create a policy: "Enable read access for all users"
   - Policy expression: `true`

### Step 2: Get API Credentials

1. Go to Project Settings → API Keys
2. Copy:
   - **Project URL**: `https://[your-project-id].supabase.co`
   - **Publishable Key**: `sb_publishable_[your-key]`

### Step 3: GenSpark AI Sheets Setup

1. Open GenSpark AI Sheets
2. Click DATA tab → DataSource
3. Click "Add Table"
4. Fill in configuration:
   - **Table Name**: `users`
   - **HTTP Method**: GET
   - **URL**: `https://[your-project-id].supabase.co/rest/v1/users?apikey=[your-publishable-key]`
   - **Format**: JSON
5. Click Add to confirm
6. Verify columns are detected (id, created_at, name, email)

### Step 4: Bind Data to Spreadsheet

1. Go to Sheet1
2. Use Sheet Binding → Load Schema
3. Data will be automatically mapped to columns
4. You can now view and edit data from Supabase

## Key Features

✅ Real-time data synchronization
✅ No backend coding required
✅ REST API based integration
✅ Secure API key authentication
✅ Full CRUD operations support
✅ Column auto-discovery

## Project Files

- `README.md` - This documentation
- `AI_Sheets-Genspark_AI_Sheets-[timestamp].xlsx` - GenSpark project file
- `SUPABASE_SETUP.md` - Detailed Supabase setup guide (optional)
- `ENV_EXAMPLE.md` - Environment variables template

## API Key Security

⚠️ **Important**: The Supabase API key in the URL is visible. For production:
- Use environment variables
- Implement backend API middleware
- Rotate keys regularly
- Use RLS policies to restrict access

## Troubleshooting

### "Invalid DataSource" Error
- Verify API key is correct
- Check URL format: `https://[project].supabase.co/rest/v1/[table-name]`
- Ensure RLS policy allows public read access
- Check browser console for detailed errors

### Columns Not Showing
- Verify the table exists in Supabase
- Check that RLS policy is enabled for read operations
- Confirm API key has correct permissions

### Data Not Loading
- Verify internet connection
- Check Supabase project is active
- Ensure table has data
- Check RLS policies

## Future Enhancements

- [ ] Add authentication UI
- [ ] Implement write/update operations
- [ ] Add data validation rules
- [ ] Create custom formulas
- [ ] Add export functionality
- [ ] Multi-table support
- [ ] Real-time subscriptions

## Resources

- [Supabase Documentation](https://supabase.io/docs)
- [GenSpark Documentation](https://www.genspark.ai)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [REST API Best Practices](https://restfulapi.net/)

## License

This project is open source and available under the MIT License.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues and questions, please open an issue on this repository.

---

**Created by**: puushkkar  
**Date**: March 14, 2026  
**Status**: ✅ Fully Functional
