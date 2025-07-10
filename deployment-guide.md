# FoodFlyers Deployment Guide

## Backend Deployment (Vercel)

### Prerequisites
1. Create accounts on:
   - [Vercel](https://vercel.com)
   - [MongoDB Atlas](https://www.mongodb.com/atlas)
   - [Stripe](https://stripe.com) (for payments)

### Step 1: Setup MongoDB Atlas
1. Create a new cluster on MongoDB Atlas
2. Create a database user with read/write permissions
3. Get your connection string (replace `<password>` with your actual password)
4. Whitelist all IP addresses (0.0.0.0/0) for production

### Step 2: Setup Environment Variables
Create a `.env` file in the backend directory with:

```env
MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/foodflyers
JWT_SECRET=your_super_secret_jwt_key_here
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key
PORT=8080
NODE_ENV=production
FRONTEND_URL=https://your-frontend-domain.vercel.app
ADMIN_URL=https://your-admin-domain.vercel.app
```

### Step 3: Deploy Backend to Vercel
1. Install Vercel CLI: `npm i -g vercel`
2. Navigate to backend directory: `cd backend`
3. Run: `vercel`
4. Follow the prompts:
   - Link to existing project? No
   - Project name: `foodflyers-backend`
   - Directory: `./` (current directory)
5. Set environment variables in Vercel dashboard:
   - Go to your project settings
   - Add all environment variables from your `.env` file

### Step 4: Deploy Frontend to Vercel
1. Navigate to frontend directory: `cd frontend`
2. Update the API URL in `src/components/Context/StoreContext.jsx`:
   ```javascript
   const url = "https://your-backend-domain.vercel.app";
   ```
3. Run: `vercel`
4. Follow the prompts for frontend deployment

### Step 5: Deploy Admin Panel to Vercel
1. Navigate to admin directory: `cd admin`
2. Update the API URL in `src/App.jsx`:
   ```javascript
   const url = "https://your-backend-domain.vercel.app"
   ```
3. Run: `vercel`
4. Follow the prompts for admin deployment

### Step 6: Update CORS Settings
After deploying frontend and admin:
1. Update your backend environment variables with the actual deployed URLs
2. Redeploy backend: `vercel --prod`

## Alternative Deployment Options

### Backend on Railway/Render
1. Connect your GitHub repository
2. Set environment variables in the platform dashboard
3. Deploy automatically from main branch

### Frontend/Admin on Netlify
1. Connect GitHub repository
2. Build command: `npm run build`
3. Publish directory: `dist`
4. Set environment variables if needed

## File Upload Configuration

For production, consider using cloud storage:
- **AWS S3**: For scalable file storage
- **Cloudinary**: For image optimization and CDN
- **Firebase Storage**: For simple file uploads

Update `backend/routes/foodRoute.js` to use cloud storage instead of local uploads.

## Database Considerations

### MongoDB Atlas Settings
- Enable MongoDB Compass access
- Set up database backups
- Monitor performance metrics
- Configure alerts for high usage

### Security Best Practices
1. Use strong JWT secrets (32+ characters)
2. Enable MongoDB Atlas IP whitelisting in production
3. Use HTTPS for all API calls
4. Implement rate limiting
5. Validate all user inputs
6. Use environment variables for all secrets

## Monitoring and Maintenance

### Recommended Tools
- **Vercel Analytics**: Monitor frontend performance
- **MongoDB Atlas Monitoring**: Database performance
- **Stripe Dashboard**: Payment monitoring
- **Sentry**: Error tracking (optional)

### Regular Maintenance
1. Update dependencies monthly
2. Monitor database storage usage
3. Review error logs weekly
4. Test payment flows regularly
5. Backup database regularly

## Troubleshooting

### Common Issues
1. **CORS Errors**: Check frontend URLs in backend CORS configuration
2. **Database Connection**: Verify MongoDB connection string and IP whitelist
3. **Payment Issues**: Check Stripe keys and webhook endpoints
4. **File Upload Errors**: Ensure upload directory exists or cloud storage is configured
5. **Authentication Issues**: Verify JWT secret consistency across deployments

### Debug Commands
```bash
# Check backend logs
vercel logs your-backend-url

# Test API endpoints
curl https://your-backend-url.vercel.app/api/food/list

# Check database connection
# Use MongoDB Compass with your connection string
```

## Production Checklist

- [ ] MongoDB Atlas cluster created and configured
- [ ] Environment variables set in all deployments
- [ ] CORS configured with production URLs
- [ ] Stripe webhooks configured (if using)
- [ ] File upload strategy implemented
- [ ] Error monitoring set up
- [ ] Database backups enabled
- [ ] SSL certificates active (automatic with Vercel)
- [ ] Performance monitoring enabled
- [ ] Security headers configured

## Support

For deployment issues:
1. Check Vercel deployment logs
2. Verify environment variables
3. Test API endpoints individually
4. Check MongoDB Atlas connection
5. Review CORS configuration

Remember to never commit `.env` files to version control!