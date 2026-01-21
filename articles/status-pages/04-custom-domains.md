---
title: Custom Domains for Status Pages
slug: custom-domains
category: Status Pages
excerpt: Use your own domain for your status page for better branding and professional appearance.
order: 4
---

# Custom Domains for Status Pages

## Default StatusApp Domain

Every status page gets a free StatusApp-hosted domain:

**Format**: `statusapp.io/{your-slug}`

**Example**: `statusapp.io/acme-status`

**Benefits**:
- No setup required
- Free SSL/HTTPS
- Works immediately
- Fully functional

**Limitations**:
- Not your branded domain
- Doesn't match your main site
- Less professional appearance

## Custom Domain Benefits

### Professional Branding
```
Default: statusapp.io/yourcompany-status
Better: status.yourcompany.com
Best: health.yourcompany.com
```

### Customer Trust
- Customers see your domain, not StatusApp's
- Appears part of your company's infrastructure
- Reinforces credibility

### SEO
- Your domain shows in search results
- Helps with brand consistency
- Improves findability

### Integration
- Matches your main website domain
- Seamless customer experience
- Professional appearance

## Setting Up Custom Domain

### Step 1: Choose Your Domain

Pick your custom domain:

**Common Options**:
- `status.yourdomain.com` (most common)
- `statuspage.yourdomain.com`
- `health.yourdomain.com`
- `incidents.yourdomain.com`
- `uptime.yourdomain.com`

**Requirements**:
- Must be a subdomain of your domain
- Can't use main domain (www.yourdomain.com)
- Subdomain must be available
- You must own/control the domain

**Decision Points**:
- Use short, memorable subdomain
- Keep consistent with brand
- Avoid technical names
- Plan for multiple status pages (separate subdomains)

### Step 2: Get CNAME Record from StatusApp

1. Go to **Settings → Status Pages**
2. Click the status page to edit
3. Look for **Custom Domain** section
4. Copy the **CNAME Record**

**Example CNAME Target**:
```
cname.statusapp.io
```

StatusApp provides the target you need to point your DNS to.

### Step 3: Configure DNS

Log into your **DNS Provider**:

Common providers:
- Cloudflare
- Route 53 (AWS)
- Namecheap
- GoDaddy
- Bluehost
- DigitalOcean

**Create CNAME Record**:

1. Access DNS management
2. Click **Add Record** or **Add DNS Record**
3. Fill in:
   - **Type**: CNAME
   - **Name/Subdomain**: `status` (the part before your domain)
   - **Target/Value**: `cname.statusapp.io` (from StatusApp)
   - **TTL**: 3600 or Auto (usually default)
4. Save record

**Example Configuration**:
```
Type:   CNAME
Name:   status
Target: cname.statusapp.io
TTL:    3600
```

**Result**: `status.yourdomain.com` will point to StatusApp

### Step 4: Update StatusApp Settings

1. Go back to status page in StatusApp
2. Enter your custom domain: `status.yourdomain.com`
3. Click **Verify Domain**
4. StatusApp validates DNS
5. Wait for confirmation

**Verification Process**:
- StatusApp checks if CNAME exists
- Checks if it points to correct target
- May take 1-2 minutes
- Green checkmark appears when verified

### Step 5: Wait for DNS Propagation

DNS changes take time to propagate globally:

**Typical Timeline**:
- Immediate (< 1 hour): In most cases
- Fast (1-4 hours): Standard timeline
- Slow (4-48 hours): Rare, depends on DNS provider

**Check Propagation Status**:
1. Use online tool: `whatsmydns.net`
2. Enter your subdomain: `status.yourdomain.com`
3. Check if CNAME record shows globally

### Step 6: SSL Certificate

StatusApp automatically provisions SSL certificate:

**Automatic Process**:
- Detects new custom domain
- Requests Let's Encrypt certificate
- Issues certificate automatically
- Renews every 90 days automatically

**Timeline**:
- Usually: 5-15 minutes after DNS propagation
- Sometimes: Up to 1 hour if DNS delayed
- No action needed from you

**Verification**:
- Browser shows padlock icon (secure)
- No SSL warnings
- Shows your domain in certificate

### Step 7: Test Custom Domain

1. Visit `https://status.yourdomain.com` in browser
2. Verify page loads
3. Check for SSL padlock icon
4. Verify your branding displays correctly
5. Test on mobile device
6. Test on different browsers

## Troubleshooting DNS Issues

### Domain Still Shows StatusApp Domain

**Check DNS Configuration**:
```
Use whatsmydns.net to verify CNAME exists
Verify CNAME points to cname.statusapp.io
Make sure TTL has expired (usually 3600 seconds = 1 hour)
```

**Wait for Propagation**:
- DNS changes take up to 48 hours globally
- Some regions faster than others
- Try from different location/network

**Verify in StatusApp**:
1. Check DNS verification status in StatusApp
2. Might show "Not Verified" if DNS not detected
3. Click verify again after DNS updates
4. Look for green checkmark

### SSL Certificate Not Issuing

**Wait Longer**:
- Takes up to 1 hour sometimes
- Try again in 5 minutes
- Check in different browser

**Verify DNS First**:
- SSL can't issue without DNS propagation
- Check DNS is fully propagated
- StatusApp needs to reach your domain

**Check Email**:
- Let's Encrypt might send verification email
- Check spam/junk folder
- May need domain owner verification

### Conflicting DNS Records

**Check for A Records**:
```
PROBLEM: Both CNAME and A record exist for same subdomain
SOLUTION: Delete A record, keep only CNAME

DNS can't have both CNAME and A record for same name
```

**MX Records (if applicable)**:
- MX records on subdomains can cause issues
- If subdomain gets mail, might need different setup
- Contact support if both needed

### CNAME Points to Wrong Target

**Verify Correct Target**:
```
StatusApp says:  cname.statusapp.io
Your DNS shows: something else

ACTION: Update your CNAME to correct target
Wait for TTL to expire and propagate
```

## Advanced Configuration

### Multiple Status Pages

Create separate custom domains for each status page:

```
Primary Status Page:
└─ status.yourdomain.com → statusapp.io/main-status

Secondary Status Page (e.g., Operations):
└─ ops-status.yourdomain.com → statusapp.io/ops-status

Incident Status Page:
└─ incidents.yourdomain.com → statusapp.io/incidents

Each with own custom domain and CNAME record
```

### Custom Subdomain per Team

Different teams with different status pages:

```
Product Team:
└─ status-product.yourdomain.com

Infrastructure Team:
└─ status-infra.yourdomain.com

Sales Team:
└─ status-sales.yourdomain.com
```

## Removing Custom Domain

To go back to StatusApp domain:

1. Go to status page settings
2. Delete or clear custom domain field
3. Click **Save**
4. Status page reverts to `statusapp.io/{slug}`
5. Can optionally remove CNAME record from DNS

**After Removal**:
- Custom domain stops working
- CNAME record in DNS becomes unused
- StatusApp domain works immediately

## Plan Requirements

Custom domains available on:
- **Free**: ✗ Not available
- **Starter**: ✗ Not available
- **Pro**: ✗ Not available
- **Business**: ✓ Available
- **Enterprise**: ✓ Available

Upgrade to Business or Enterprise plan to use custom domains.

## Best Practices

### 1. Use Recognizable Subdomain

```
Good: status.company.com (easy to remember)
Good: health.company.com (professional)
Bad: sp1.company.com (confusing)
Bad: asdf.company.com (unprofessional)
```

### 2. Plan for Scale

```
If multiple status pages planned:
├─ status.yourdomain.com (main)
├─ ops-status.yourdomain.com (operations)
└─ incident-status.yourdomain.com (incidents)

Use descriptive subdomains
Reserve main 'status' for primary page
```

### 3. Communicate to Customers

After setting up custom domain:
1. Update website links to use new domain
2. Update documentation and runbooks
3. Update email communications
4. Share new URL with customers
5. Old StatusApp domain still works (can redirect)

### 4. Monitor SSL Certificate

```
StatusApp handles renewal automatically
But worth checking occasionally:
- Certificate details in browser
- Matches your domain
- No expiration warnings
- Renew works every 90 days
```

### 5. Keep CNAME Record

```
Keep DNS record active permanently:
- Status page relies on it
- Removing breaks custom domain
- Easy to add back if removed
- Low cost to maintain
```

## Testing Checklist

Before making custom domain public:

- [ ] Custom domain DNS verified in StatusApp
- [ ] Domain loads in browser (HTTPS)
- [ ] SSL certificate shows (padlock icon)
- [ ] No security warnings
- [ ] Branding displays correctly
- [ ] Works on mobile
- [ ] Works on different browsers
- [ ] Email notifications still send
- [ ] All monitors display correctly
- [ ] Incidents display properly

## FAQ

**Q: Can I use my main domain (www.yourdomain.com)?**
A: No, custom domains must be subdomains. Use status.yourdomain.com instead.

**Q: Will my current URL stop working?**
A: No, StatusApp domain continues working. Custom domain is in addition, not replacement.

**Q: Can I change custom domains later?**
A: Yes, update DNS and StatusApp settings. Both domains work during transition.

**Q: How long until SSL certificate is issued?**
A: Usually 5-15 minutes after DNS propagation completes.

**Q: Do I need to renew SSL manually?**
A: No, StatusApp handles renewal automatically every 90 days.

**Q: What if I remove my custom domain?**
A: Status page reverts to StatusApp domain immediately. You can re-add custom domain anytime.

## Next Steps

- **[Creating Status Pages](/articles/status-pages/creating-status-pages)** - Set up your status page
- **[Embedding Status Pages](/articles/status-pages/embedding-status-pages)** - Embed on your website
- **[Incidents](/articles/incidents/incident-management-basics)** - Manage incidents on status page
