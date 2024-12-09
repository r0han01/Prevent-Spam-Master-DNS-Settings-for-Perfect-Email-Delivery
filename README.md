# Prevent Spam: Master DNS Settings for Perfect Email Delivery

When you purchase a domain for your business, it's important to configure DNS records properly to prevent your emails from being marked as spam. Follow these steps to configure your DNS records using Cloudflare (or a similar DNS provider).

### Prerequisites
- You must have a domain registered with a hosting provider (e.g., GoDaddy, Namecheap, etc.).
- You will need to sign up for a Cloudflare account if you haven't already.

### Step 1: Add Your Domain to Cloudflare
1. Go to [Cloudflare](https://www.cloudflare.com/).
2. Sign up or log in to your Cloudflare account.
3. Add your domain by entering it into Cloudflare’s "Add a Site" section.
4. Follow the instructions provided by Cloudflare to change your domain’s nameservers to the ones provided by Cloudflare.

### Step 2: Add Required DNS Records
Once your domain is connected to Cloudflare, you’ll need to add the following DNS records to ensure proper email deliverability.

#### Example Domain: `example.com`

#### A Records
- **Name**: `mail`
- **Content**: `198.51.100.1` (This is a random IP address. Replace it with your actual mail server's IP address if different)
- **Proxy status**: `DNS only`
- **TTL**: Auto (Cloudflare automatically sets TTL to Auto)

Repeat the same for:
- **Name**: `example.com`
- **Content**: `198.51.100.1` (This is a random IP address. Replace it with your actual mail server's IP address)
- **Proxy status**: `DNS only`

#### CNAME Record (For "www" subdomain)
- **Name**: `www`
- **Content**: `example.com` (Replace `example.com` with your domain name)
- **Proxy status**: `Proxied`
- **TTL**: Auto

#### MX Record (Mail Exchanger)
- **Name**: `example.com` (Replace with your domain name)
- **Content**: `mail.example.com` (Replace with your mail server address)
- **Priority**: `10` (Ensure this is set to priority 10 or as specified by your mail provider)
- **Proxy status**: `DNS only`
- **TTL**: Auto

#### TXT Record for DMARC (Domain-based Message Authentication, Reporting, and Conformance)
- **Name**: `_dmarc`
- **Content**: `"v=DMARC1; p=quarantine; rua=mailto:postmaster@example.com"` (Replace `example.com` with your domain name)
- **Proxy status**: `DNS only`
- **TTL**: Auto

#### TXT Record for SPF (Sender Policy Framework)
- **Name**: `example.com`
- **Content**: `"v=spf1 ip4:198.51.100.1 ~all"` (Replace `198.51.100.1` with your mail server's IP address)
- **Proxy status**: `DNS only`
- **TTL**: Auto

### Step 3: Verify Your DNS Configuration
After adding all the necessary records to Cloudflare (or your DNS provider), allow some time for the changes to propagate (usually 30 minutes to 1 hour). You can verify the correct configuration using tools like:
- [MXToolbox](https://mxtoolbox.com/)
- [DNSstuff](https://www.dnsstuff.com/)

Check that:
- Your `A`, `MX`, `TXT`, and `CNAME` records are correctly set.
- There are no conflicting records that might cause issues with your email deliverability.

### Step 4: Test Email Deliverability
Send test emails to different email providers (like Gmail, Yahoo, Outlook) to ensure that:
- Your email does not end up in the spam folder.
- SPF, DKIM, and DMARC records are validated correctly.

### Additional Notes
- **SPF Record**: Helps prevent email spoofing by specifying which mail servers are allowed to send email on behalf of your domain.
- **DMARC Record**: Defines the policy for handling emails that fail SPF or DKIM checks.
- **MX Record**: Specifies the mail server for receiving emails for your domain.
- **CNAME Record**: Allows you to alias your domain to another domain (for `www` subdomains).

### Example DNS Setup for `example.com`
Here’s what your DNS setup might look like in Cloudflare:

| Type | Name          | Content               | Proxy status | TTL   |
|------|---------------|-----------------------|--------------|-------|
| A    | mail          | 198.51.100.1          | DNS only     | Auto  |
| A    | example.com   | 198.51.100.1          | DNS only     | Auto  |
| CNAME| www           | example.com           | Proxied      | Auto  |
| MX   | example.com   | mail.example.com      | DNS only     | Auto  |
| TXT  | _dmarc        | "v=DMARC1; p=quarantine; rua=mailto:postmaster@example.com" | DNS only | Auto |
| TXT  | example.com   | "v=spf1 ip4:198.51.100.1 ~all" | DNS only | Auto |

### Troubleshooting
- If emails are still going to spam, double-check your SPF and DMARC records.
- Make sure that Cloudflare is set to **DNS only** for email-related records.
- Check the email headers to see why emails are being flagged as spam. Look for SPF or DKIM failures.

### Conclusion
By following this guide and configuring DNS records for your domain using Cloudflare, you'll improve the chances of your business emails reaching the recipient's inbox instead of the spam folder. Always verify your records and test email deliverability after making changes.
