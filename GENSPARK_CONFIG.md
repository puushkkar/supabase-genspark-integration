# GenSpark AI Sheets Configuration Guide

Step-by-step guide to connect Supabase to GenSpark AI Sheets and configure DataSource binding.

## Prerequisites

- GenSpark AI Sheets access
- Completed Supabase setup (see SUPABASE_SETUP.md)
- Supabase credentials:
  - Project URL
  - Publishable API Key
  - Table name (e.g., `users`)

## Configuration Steps

### Step 1: Open GenSpark AI Sheets

1. Navigate to [GenSpark AI Sheets](https://www.genspark.ai/agents?type=sheets_agent_new)
2. Create a new project or open existing one
3. You should see an empty spreadsheet with tabs: INSERT, DATA, VIEW, SETTINGS

### Step 2: Access DataSource Configuration

1. Click the **DATA** tab at the top
2. In the left panel, click **DataSource** button
3. The DataSource panel will open on the right side
4. You should see:
   - "Tables" section
   - "Add Table" button
   - Table configuration area

### Step 3: Add New Table

1. Click the **"Add Table"** button
2. A new table entry will appear
3. You'll see the table list on the left with your new table

### Step 4: Configure Table Settings

#### Table Name
1. Find the **"Table Name"** field
2. Enter your Supabase table name (e.g., `users`)
3. This should match exactly with Supabase table name

#### HTTP Method and URL
1. Locate the **"Read"** section
2. HTTP Method dropdown should show: **GET**
3. In the **URL** field, enter:
   ```
   https://[YOUR_PROJECT].supabase.co/rest/v1/[TABLE_NAME]?apikey=[YOUR_PUBLISHABLE_KEY]
   ```

**Example:**
```
https://abcdefghijklmno.supabase.co/rest/v1/users?apikey=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Where:**
- `[YOUR_PROJECT]` = Your Supabase project ID
- `[TABLE_NAME]` = Table name (users, products, etc.)
- `[YOUR_PUBLISHABLE_KEY]` = Supabase publishable API key

#### Data Format
1. Below the URL, find **"Format"** dropdown
2. Select **JSON** (default)
3. This should match your Supabase API response format

### Step 5: Auto Sync Configuration

1. Locate **"Auto Sync"** checkbox
2. Check if you want automatic data refresh
3. Optional: Set sync interval if available

### Step 6: Verify Connection

1. Click the **"Columns"** tab in the DataSource panel
2. You should see your table columns listed:
   - id
   - created_at
   - name
   - email
   (or whatever columns your table has)

3. If columns don't appear:
   - Check URL is correct
   - Verify API key is valid
   - Ensure RLS policy allows public read access
   - Check browser console for errors (F12)

### Step 7: Bind Data to Spreadsheet

1. Close the DataSource panel
2. Go to **Sheet1** tab
3. Click the **DATA** tab again
4. Click **Sheet Binding**
5. Choose **"Load Schema"**
6. Wait for data to load (usually 2-3 seconds)
7. Columns should now be populated with your Supabase data

### Step 8: Customize Spreadsheet

Now you can:
- Add headers (row 1)
- Format columns
- Add formulas
- Create calculated fields
- Style cells

## Advanced Configurations

### Configure Update Operations

To enable updating data back to Supabase:

1. In DataSource, find the **"Update"** section
2. HTTP Method: **PUT**
3. URL pattern:
   ```
   https://[PROJECT].supabase.co/rest/v1/[TABLE]?id=eq.[ID_VALUE]&apikey=[KEY]
   ```

### Configure Delete Operations

To enable deletion:

1. Find the **"Delete"** section
2. HTTP Method: **DELETE**
3. URL:
   ```
   https://[PROJECT].supabase.co/rest/v1/[TABLE]?id=eq.[ID_VALUE]&apikey=[KEY]
   ```

### Configure Create Operations

To enable adding new rows:

1. Find the **"Create"** section
2. HTTP Method: **POST**
3. URL:
   ```
   https://[PROJECT].supabase.co/rest/v1/[TABLE]?apikey=[KEY]
   ```

## Common Issues and Solutions

### Issue: "Invalid DataSource" Error

**Causes & Solutions:**
- ✓ Check URL format - must include `/rest/v1/` path
- ✓ Verify API key is correct
- ✓ Ensure table name is spelled correctly
- ✓ Check RLS policy allows public read
- ✓ Try accessing URL directly in browser

### Issue: Columns Not Showing

**Causes & Solutions:**
- ✓ Click "Columns" tab and wait 2-3 seconds
- ✓ Verify table has data in Supabase
- ✓ Check RLS policy:
  - Go to Supabase > Authentication > Policies
  - Ensure SELECT policy exists for your table
- ✓ Test API endpoint in browser:
  ```
  https://your-project.supabase.co/rest/v1/users?apikey=your-key
  ```
  You should see JSON data

### Issue: CORS Errors

**Solution:**
1. Go to Supabase Project Settings > API
2. Find "CORS settings"
3. Add `https://www.genspark.ai` to allowed origins
4. Or add `*` for testing (not recommended for production)

### Issue: "Unauthorized" or "403 Forbidden"

**Causes & Solutions:**
- ✓ RLS policy might be too restrictive
- ✓ API key might be secret key instead of publishable key
- ✓ Try updating RLS policy to use `auth.uid() = id` is less restrictive

### Issue: Slow Data Loading

**Solutions:**
- ✓ Disable "Auto Sync" if not needed
- ✓ Increase pagination/limit in URL
- ✓ Add indexes to frequently queried columns
- ✓ Try filtering data: `?limit=100&offset=0`

## Exporting and Sharing

### Export as Excel

1. Click **Export** button (top left)
2. Choose export format
3. File will download to your computer

### Export Configuration

The project file (.xlsx) contains:
- Spreadsheet data
- DataSource configuration
- Formulas and formatting
- Binding information

## Performance Tips

1. **Pagination**: Use `?limit=100&offset=0` for large datasets
2. **Filtering**: Filter at API level, not in spreadsheet
3. **Caching**: Use Auto Sync sparingly
4. **Indexing**: Add database indexes for frequently queried columns
5. **Compression**: Keep payload small

## Security Best Practices

⚠️ **API Key Visibility**: Your publishable key is visible in the URL

For production:
1. Use restrictive RLS policies
2. Implement row-level access control
3. Use service role key for server-side operations
4. Consider API gateway/middleware
5. Enable audit logging in Supabase

## Troubleshooting Checklist

- [ ] Supabase project is active and not paused
- [ ] Table exists and has data
- [ ] RLS policy allows SELECT for your user
- [ ] API key is correct (not secret key)
- [ ] URL format is correct with `/rest/v1/`
- [ ] CORS is configured if getting browser errors
- [ ] Columns appear in DataSource > Columns tab
- [ ] Data appears in spreadsheet after binding
- [ ] Network requests show 200 status (F12 > Network)
- [ ] No JavaScript errors in console (F12)

## Support Resources

- GenSpark Documentation: https://www.genspark.ai
- Supabase REST API: https://supabase.io/docs/reference/api
- Supabase RLS Guide: https://supabase.io/docs/learn/auth-deep-dive/row-level-security
- Community Help: https://github.com/supabase/supabase/discussions

## Next Steps

After successful configuration:

1. Add more tables if needed
2. Create relationships between tables
3. Add calculated columns
4. Set up data validation
5. Create dashboards and reports
6. Share project with team members

---

**Last Updated**: March 14, 2026
**GenSpark Version**: Latest
**Status**: ✅ Tested and Working
