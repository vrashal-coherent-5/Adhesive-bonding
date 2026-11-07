# Auth0 setup for the Adhesives & Bonding Portal

## 1. Application settings (Applications → Insight_Vault → Settings)
- **Application Type:** Single Page Application (the portal is a static page and signs in with PKCE, so it uses no client secret)
- **Allowed Callback URLs / Allowed Logout URLs / Allowed Web Origins:** your site URL, e.g.
  `https://your-portal.vercel.app/` (add `http://localhost:5500/` too if you test locally)
- Save.

## 2. Put roles into the login token (Actions → Library → Create Action → "Login / Post Login")
```js
exports.onExecutePostLogin = async (event, api) => {
  const roles = (event.authorization && event.authorization.roles) || [];
  api.idToken.setCustomClaim('https://adhesive-portal/roles', roles);
  api.accessToken.setCustomClaim('https://adhesive-portal/roles', roles);
};
```
Deploy it, then add it to the **Login** flow (Actions → Triggers → post-login) and Apply.

## 3. Assign roles
User Management → Users → pick a user → Roles → Assign Roles. A user with no role sees "no access role is assigned".

To let only specific roles in, edit `allowedRoles` in `index.html`, e.g. `allowedRoles: ['Admin', 'Viewer']`.
