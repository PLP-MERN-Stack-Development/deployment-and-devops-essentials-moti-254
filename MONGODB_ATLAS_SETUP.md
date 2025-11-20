# MongoDB Atlas Setup Guide

## Step 1: Create a MongoDB Atlas Account
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Click "Sign Up" and create your account
3. Verify your email address

## Step 2: Create a New Cluster
1. Click "Create" to build a new database
2. Select **M0 (Free)** tier for development/testing
3. Choose your cloud provider (AWS, Google Cloud, or Azure)
4. Select a region closest to your deployment location
5. Click "Create Cluster" (takes 1-3 minutes)

## Step 3: Create Database User
1. Go to **Database Access** in the left sidebar
2. Click **Add New Database User**
3. Create a username and strong password
4. Assign role: **Atlas admin** or **Read and write to any database**
5. Click **Add User**

**Save these credentials securely!**

## Step 4: Set Up Network Access
1. Go to **Network Access** in the left sidebar
2. Click **Add IP Address**
3. For development: Add your IP address
4. For production (Render): Click **Allow Access from Anywhere** (0.0.0.0/0)
   - This is safe if you use strong database user credentials
5. Click **Confirm**

## Step 5: Get Connection String
1. Go back to **Databases**
2. Click **Connect** on your cluster
3. Select **Drivers** > **Node.js**
4. Copy the connection string
5. Replace `<password>` with your database user password
6. Replace `<database>` with your database name

### Connection String Format:
```
mongodb+srv://username:password@cluster.mongodb.net/databasename?retryWrites=true&w=majority
```

## Step 6: Configure in Your Application
Add to your `.env` file:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/databasename?retryWrites=true&w=majority
```

## Step 7: Verify Connection
Test the connection string in your application by logging successful connections:
```javascript
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

mongoose.connection.on('connected', () => {
  console.log('MongoDB connected successfully');
});

mongoose.connection.on('error', (err) => {
  console.error('MongoDB connection error:', err);
});
```

## Security Best Practices
✅ Use strong database passwords (mix of upper/lowercase, numbers, symbols)
✅ Create separate database users for dev and production
✅ Limit IP access where possible
✅ Never commit connection strings to git
✅ Use environment variables for sensitive data
✅ Regularly monitor database activity in Atlas dashboard

## Troubleshooting
- **Connection timeout?** Check Network Access settings (IP whitelisting)
- **Authentication failed?** Verify username/password and special characters are URL-encoded
- **Database not found?** Ensure database name matches in connection string
- **Too many connections?** Check connection pooling settings in your driver
