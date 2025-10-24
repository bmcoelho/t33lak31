# Tesla Fleet API – Public Key Hosting for Home Assistant

This repository hosts the **public key** required to verify a custom domain for the [Tesla Fleet API](https://developer.tesla.com/) when integrating with [Home Assistant](https://www.home-assistant.io/).

The file is served at:

```
https://tesla.gninah.com/.well-known/appspecific/com.tesla.3p.public-key.pem
```

✅ **Important:** Only the **public** key is stored here. It’s safe and expected to be publicly accessible.  
❌ I do **not** store any client secrets or sensitive tokens in this repository.

---

## 🧰 Prerequisites

- A Tesla Developer account
- A registered app in the Tesla Developer Portal
- A GitHub account
- A custom domain (e.g. `gninah.com`) managed through your DNS provider
- A running [Home Assistant](https://www.home-assistant.io/) instance with the Tesla Fleet integration

---

## 🪄 Step-by-Step Configuration

### 1. 📁 Prepare the Repository

- Create a new **public** GitHub repository.  
- Add the following folder structure to the root of the repo:
  ```
  .nojekyll
  CNAME
  /.well-known/appspecific/com.tesla.3p.public-key.pem
  ```

- `CNAME` file should contain your subdomain:
  ```
  tesla.gninah.com
  ```

- The `.nojekyll` file must be empty. It ensures GitHub Pages publishes the `.well-known` folder.

---

### 2. 🌐 Configure DNS

- In your DNS provider (e.g. GoDaddy), create a CNAME record:
  ```
  Type:  CNAME
  Name:  tesla
  Value: <your-github-username>.github.io
  TTL:   600
  ```

Wait a few minutes for DNS to propagate.  
You can verify with:
```
dig tesla.gninah.com CNAME +short
```

---

### 3. 🚀 Enable GitHub Pages

- Go to **Repository Settings → Pages**.
- Source: `Deploy from a branch`
- Branch: `main`
- Folder: `/`
- Set the **custom domain** to:
  ```
  tesla.gninah.com
  ```
- ✅ DNS Check should show **successful**.
- Enable **Enforce HTTPS**.

Your PEM file should now be accessible at:
```
https://tesla.gninah.com/.well-known/appspecific/com.tesla.3p.public-key.pem
```

---

### 4. ⚡ Configure Tesla Developer App

In the [Tesla Developer Portal](https://developer.tesla.com/):

- **Allowed Origin URL(s)**:
  ```
  https://tesla.gninah.com/
  ```
- **Allowed Redirect URI(s)**:
  ```
  https://my.home-assistant.io/redirect/oauth
  ```
- **OAuth Grant Types**:
  - ✅ `authorization_code`
  - ✅ `client_credentials` (Machine-to-Machine)
- **Scopes**:
  - Vehicle Information *(at minimum)*
  - Add additional scopes as needed (e.g. Vehicle Commands, Location, Energy, etc.)

Save the configuration.

---

### 5. 🏠 Configure Home Assistant

- Go to **Settings → Devices & Services → Application Credentials**.
- Add a new credential for **Tesla Fleet** with your Client ID and Secret from the Tesla Developer Portal.
- Add the Tesla Fleet integration.
- When asked for the **domain**, enter:
  ```
  tesla.gninah.com
  ```
- Complete the Tesla login and approval flow.

✅ Tesla will verify your PEM file hosted on GitHub Pages, and the integration will complete successfully.

---

## 🛡️ Security Notes

- This repo only contains your **public key**.  
  It’s safe to keep it public.
- I never store:
  - Tesla client secrets
  - Access tokens
  - Private keys
- If you ever rotate your key in Home Assistant, replace the file here with the new PEM.

---

## 🧪 Troubleshooting

- **404 error on PEM URL**  
  → Make sure `.nojekyll` exists at the repo root and your folder is exactly named `.well-known`.

- **HTTPS “Unavailable”**  
  → Check DNS is pointing to `yourusername.github.io` and remove any conflicting A records.

- **Tesla OAuth errors**  
  → Confirm only one domain is listed in **Allowed Origin URLs** and it matches exactly what you entered in HA.

---

## 📎 Useful Links

- [Tesla Developer Portal](https://developer.tesla.com/)
- [Home Assistant Tesla Fleet Integration Docs](https://www.home-assistant.io/integrations/tesla_fleet/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [DNS Checker](https://dnschecker.org)

---

✨ *This repository is only used to host a public key for Tesla domain verification. Nothing else should be stored here.*
