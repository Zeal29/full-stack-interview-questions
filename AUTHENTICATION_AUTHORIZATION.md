# Authentication & Authorization Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) | Intermediate (Q21–Q40) | Advanced (Q41–Q50)

---

## Basic Questions (Q1–Q20)

### Q1. What is the difference between authentication and authorization?

**Answer:**
- **Authentication** (AuthN): "Who are you?" — verifying identity (login, password check)
- **Authorization** (AuthZ): "What can you do?" — checking permissions (admin? can delete?)

Authentication happens first, then authorization. You must be authenticated to be authorized.

**💡 Tip:** Authentication = **"who are you?"** Authorization = **"what are you allowed to do?"** AuthN → AuthZ. Login → Permissions.

---

### Q2. What is JWT (JSON Web Token)?

**Answer:** JWT is a compact, self-contained token for securely transmitting information between parties. Structure: `header.payload.signature` (three Base64URL strings separated by dots).
- **Header**: algorithm + type (`{"alg": "HS256", "typ": "JWT"}`)
- **Payload**: claims/data (`{"sub": "123", "name": "Ali", "exp": 1234567890}`)
- **Signature**: verifies token integrity (`HMACSHA256(base64(header) + "." + base64(payload), secret)`)

**💡 Tip:** JWT = **sealed envelope** with data. Header = how it's sealed. Payload = the letter. Signature = wax seal (proves it wasn't tampered with).

---

### Q3. What are the types of JWT claims?

**Answer:**
- **Registered**: `iss` (issuer), `sub` (subject), `aud` (audience), `exp` (expiration), `iat` (issued at), `nbf` (not before)
- **Public**: custom claims with collision-resistant names
- **Private**: custom claims for your app (`role`, `userId`)

Keep payload small — token sent with every request.

**💡 Tip:** `exp` = **most important** (when it expires). `sub` = who it's for. `iat` = when created. Keep payload minimal — smaller token = faster requests.

---

### Q4. What is OAuth 2.0?

**Answer:** OAuth 2.0 is an authorization framework that allows third-party applications to access user data without sharing passwords. Defines flows (grant types) for different scenarios:
- **Authorization Code**: web apps (most secure)
- **Authorization Code + PKCE**: mobile/SPAs
- **Client Credentials**: server-to-server
- **Implicit**: legacy (avoid — use PKCE instead)
- **Resource Owner Password**: legacy (avoid)

**💡 Tip:** OAuth2 = **"let this app access my data without giving my password."** Authorization Code flow = most common. Use PKCE for mobile/SPAs.

---

### Q5. What is the Authorization Code flow?

**Answer:** Most secure OAuth2 flow for server-side apps:
1. Client redirects user to authorization server
2. User logs in and grants permission
3. Auth server redirects back with **authorization code**
4. Client exchanges code for **access token** (server-to-server)
5. Client uses access token to call APIs

Step 4 happens server-to-server — code can't be stolen from browser.

**💡 Tip:** Authorization Code = **two-step**: browser gets code, server exchanges code for token. Code is single-use, short-lived. Tokens never exposed to browser during exchange.

---

### Q6. What is an access token vs refresh token?

**Answer:**
- **Access token**: short-lived (15 min - 1 hour), used for API calls, included in `Authorization: Bearer <token>`
- **Refresh token**: long-lived (7-30 days), used ONLY to get new access tokens, stored securely (httpOnly cookie or secure storage)

When access token expires, use refresh token to get a new one — no re-login needed.

**💡 Tip:** Access token = **"today's pass"** (short-lived, used everywhere). Refresh token = **"renewal card"** (long-lived, only used at token endpoint). If access token leaks: minimal damage (expires soon).

---

### Q7. What is session-based authentication?

**Answer:** Server stores session data, client receives session ID in cookie:
1. User logs in → server creates session (in memory/Redis/database)
2. Server sends session ID in `Set-Cookie` header
3. Client sends cookie with every request
4. Server looks up session by ID

**💡 Tip:** Session = **server remembers you** (stored server-side). JWT = **you carry your ID** (stored client-side). Sessions need shared storage for scaling. JWTs are stateless.

---

### Q8. What is the difference between JWT and sessions?

**Answer:**
| Feature | JWT | Sessions |
|---|---|---|
| Storage | Client (token) | Server (memory/Redis) |
| State | Stateless | Stateful |
| Scaling | Easy (no shared state) | Needs shared session store |
| Revocation | Hard (until expiry) | Easy (delete session) |
| Size | Large (contains claims) | Small (just session ID) |
| Best for | APIs, microservices | Traditional web apps |

**💡 Tip:** JWT = **stateless, self-contained** (good for APIs/distributed). Sessions = **stateful, server-stored** (easier to revoke). Choose based on architecture.

---

### Q9. What is RBAC (Role-Based Access Control)?

**Answer:** Permissions assigned to roles, users assigned to roles:
```javascript
const roles = {
  admin: ['read', 'write', 'delete', 'manage_users'],
  editor: ['read', 'write'],
  viewer: ['read']
};
// Check: user.role === 'admin' → can delete
```

Simple, common, works for most apps. Roles: admin, user, moderator, etc.

**💡 Tip:** RBAC = **"your role decides what you can do."** Like job titles → permissions. User role = basic access. Admin role = everything. Simple and effective.

---

### Q10. What is ABAC (Attribute-Based Access Control)?

**Answer:** Permissions based on attributes (user, resource, environment):
```javascript
// Can user edit this document?
canEdit = user.department === resource.department && 
          user.clearance >= resource.requiredClearance &&
          time.isBusinessHours();
```

More flexible than RBAC but more complex. Used in: healthcare, finance, government.

**💡 Tip:** ABAC = **"context decides."** User attributes + resource attributes + environment = decision. More flexible than RBAC but harder to manage. Use for fine-grained control.

---

### Q11. What is hashing vs encryption?

**Answer:**
- **Hashing**: one-way function. Input → fixed output. Can't reverse. Used for passwords. (`bcrypt`, `scrypt`, `argon2`)
- **Encryption**: two-way. Encrypt with key → decrypt with key. Used for data at rest/transit. (`AES`, `RSA`)

**💡 Tip:** Hash = **one-way blender** (can't un-blend). Encrypt = **lockbox** (key opens it). Passwords → hash. Credit cards → encrypt. Never encrypt passwords.

---

### Q12. What is bcrypt and why use it?

**Answer:** Password hashing algorithm designed to be slow (intentionally):
- Built-in salt (prevents rainbow table attacks)
- Configurable cost factor (slower = harder to crack)
- Resistant to GPU/ASIC attacks

```javascript
const hash = await bcrypt.hash(password, 12); // cost factor 12
const match = await bcrypt.compare(inputPassword, hash);
```

**💡 Tip:** bcrypt = **"slow on purpose"** hash. Slower = harder to brute force. Cost factor 10-12 is standard. Never use MD5/SHA for passwords.

---

### Q13. What is a salt in password hashing?

**Answer:** Random data added to password before hashing to prevent:
- Rainbow table attacks
- Same password → same hash detection
- Pre-computed hash lookups

```javascript
// bcrypt auto-generates salt
hash = bcrypt.hash("password123", 10); // salt embedded in hash
```

**💡 Tip:** Salt = **"unique seasoning per user."** Same password + different salt = different hash. bcrypt auto-handles salting — don't do it manually.

---

### Q14. What is CSRF (Cross-Site Request Forgery)?

**Answer:** Attack where malicious site makes authenticated requests to your API using the victim's session cookie:
- Prevention: CSRF tokens, SameSite cookies, checking Origin/Referer headers
- Modern defense: `SameSite=Strict` or `SameSite=Lax` cookies

**💡 Tip:** CSRF = **"tricking your browser into making requests you didn't intend."** Defense: `SameSite` cookies + CSRF tokens. If using JWT in Authorization header (not cookies), CSRF isn't a threat.

---

### Q15. What is XSS (Cross-Site Scripting)?

**Answer:** Attack where malicious scripts are injected into web pages:
- **Stored XSS**: script saved in database, served to other users
- **Reflected XSS**: script in URL parameter, reflected in response
- **DOM XSS**: script manipulates page DOM

Prevention: input sanitization, output encoding, Content Security Policy (CSP), `httpOnly` cookies.

**💡 Tip:** XSS = **"injecting malicious scripts."** Sanitize all input, encode output, use CSP headers. Store JWT in httpOnly cookies — not accessible via JavaScript.

---

### Q16. What is HTTPS/TLS?

**Answer:** HTTPS encrypts all data between client and server using TLS (Transport Layer Security):
- Asymmetric encryption for key exchange (RSA, ECDHE)
- Symmetric encryption for data (AES)
- Certificate verification (trust chain)

Non-negotiable for any API handling authentication.

**💡 Tip:** HTTPS = **encrypted pipe** between client and server. No HTTPS = everything visible in transit. Always use HTTPS. Free certificates via Let's Encrypt.

---

### Q17. What is multi-factor authentication (MFA)?

**Answer:** Require multiple verification factors:
- **Something you know**: password, PIN
- **Something you have**: phone (SMS OTP, TOTP app), security key
- **Something you are**: fingerprint, face recognition

TOTP (Time-based One-Time Password): Google Authenticator, Authy. Uses shared secret + current time.

**💡 Tip:** MFA = **"prove it two ways."** Password + phone. Even if password leaks, attacker needs the phone. Always offer MFA. TOTP > SMS (SIM swapping attacks).

---

### Q18. What is an API key?

**Answer:** Simple string identifier for API access:
```
Authorization: ApiKey abc123def456
```

Pros: simple to implement. Cons: no user context, no fine-grained permissions, can't expire easily, often sent in URL (logged). Use for: server-to-server, third-party integrations.

**💡 Tip:** API key = **"club membership card"** (identifies, doesn't authenticate a person). Good for server-to-server. Bad for user authentication. Use JWT for user auth.

---

### Q19. What is a man-in-the-middle (MITM) attack?

**Answer:** Attacker intercepts communication between client and server:
- Without HTTPS: attacker reads/modifies everything
- With HTTPS: extremely difficult (certificate validation)
- Defense: HTTPS, certificate pinning, HSTS header

**💡 Tip:** MITM = **"eavesdropper between you and the server."** HTTPS prevents it. HSTS forces HTTPS. Always verify certificates. Never ignore certificate errors.

---

### Q20. What is passwordless authentication?

**Answer:** Login without passwords using:
- **Magic links**: email with one-time login link
- **OTP**: one-time code via email/SMS
- **WebAuthn/FIDO2**: biometric or hardware key
- **Passkeys**: device-based authentication (fingerprint, face)

Eliminates password-related attacks (phishing, credential stuffing).

**💡 Tip:** Passwordless = **"no password to steal."** Magic link, OTP, fingerprint. More secure + better UX. The future of authentication.

---

## Intermediate Questions (Q21–Q40)

### Q21. How does token rotation work?

**Answer:** On each refresh token use:
1. Validate refresh token
2. Issue new access token + NEW refresh token
3. Invalidate old refresh token

If old refresh token is reused → both tokens are revoked (indicates theft). Detects token theft.

**💡 Tip:** Rotation = **"use once, get a new one."** Like numbered tickets — if someone uses your old ticket, both are voided. Detects stolen refresh tokens.

---

### Q22. What is PKCE (Proof Key for Code Exchange)?

**Answer:** Extension to OAuth2 for public clients (SPAs, mobile):
1. Client generates random `code_verifier` and `code_challenge` (SHA256 hash)
2. Sends `code_challenge` with authorization request
3. On token exchange, sends original `code_verifier`
4. Server verifies hash matches

Prevents authorization code interception.

**💡 Tip:** PKCE = **"prove you started the login."** Code challenge = "here's what I'll prove later." Code verifier = "here's my proof." Required for mobile/SPAs.

---

### Q23. What is OpenID Connect (OIDC)?

**Answer:** Identity layer on top of OAuth 2.0:
- OAuth2 = authorization (access to resources)
- OIDC = authentication (who is the user?)

Adds: `id_token` (JWT with user info), `/userinfo` endpoint, standard claims (name, email, picture). Used by: Google Login, Sign in with Apple, Auth0.

**💡 Tip:** OIDC = **OAuth2 + identity**. OAuth2 = "can I access your data?" OIDC = "who are you?" Google/Facebook Login uses OIDC.

---

### Q24. What is the `sub` claim in JWT?

**Answer:** `sub` (subject) uniquely identifies the user/entity the token represents. Must be unique within the issuer's scope. Should NOT contain email/username (can change) — use immutable identifier (database ID, UUID).

**💡 Tip:** `sub` = **"who this token belongs to."** Use immutable IDs, not emails (emails change). Standard claim understood by all JWT libraries.

---

### Q25. What is the difference between symmetric and asymmetric JWT signing?

**Answer:**
- **Symmetric (HS256)**: same secret for signing and verification. Both parties share the secret.
- **Asymmetric (RS256/ES256)**: private key signs, public key verifies. Only auth server has private key.

RS256 is preferred for distributed systems — services verify tokens without knowing the signing secret.

**💡 Tip:** HS256 = **one shared secret** (simpler, both sides know secret). RS256 = **public/private key pair** (more secure, services only have public key). Use RS256 for microservices.

---

### Q26. How to store tokens securely?

**Answer:**
| Storage | Access Token | Refresh Token |
|---|---|---|
| **httpOnly cookie** | ✅ Best for web | ✅ Best for web |
| **localStorage** | ⚠️ Vulnerable to XSS | ❌ Never |
| **sessionStorage** | ⚠️ Vulnerable to XSS | ❌ Never |
| **Memory (JS variable)** | ✅ Best (lost on refresh) | ❌ Not practical |
| **Secure storage (mobile)** | ✅ Keychain/Keystore | ✅ Keychain/Keystore |

**💡 Tip:** httpOnly + Secure + SameSite cookies = **best for web** (JavaScript can't access). localStorage = XSS vulnerable. Mobile = platform secure storage.

---

### Q27. What is CORS in the context of authentication?

**Answer:** CORS + Auth specifics:
- `Access-Control-Allow-Credentials: true` — allow cookies/auth headers
- `Access-Control-Allow-Origin` must be specific (not `*`) when credentials are true
- Preflight requests (OPTIONS) must be handled
- Expose headers: `Access-Control-Expose-Headers: Authorization`

**💡 Tip:** With cookies/auth: CORS must allow credentials + specific origin (not `*`). Handle OPTIONS preflight. Expose custom auth headers.

---

### Q28. What is privilege escalation?

**Answer:** Attack where user gains higher permissions than intended:
- **Vertical**: regular user → admin (modify role in JWT/cookie)
- **Horizontal**: user A → access user B's data (change ID parameter)

Prevention: server-side authorization checks, never trust client-side role claims alone.

**💡 Tip:** Privilege escalation = **"becoming someone more powerful."** Vertical = higher role. Horizontal = different user. Always verify permissions server-side.

---

### Q29. What is a bearer token?

**Answer:** "Whoever bears this token has access" — no proof of ownership beyond possession:
- Sent in: `Authorization: Bearer <token>`
- Risk: if stolen, attacker has full access
- Mitigation: short expiration, HTTPS only, secure storage

**💡 Tip:** Bearer = **"finder's keepers"** — whoever has it, uses it. No additional proof needed. That's why short expiration + secure storage matters.

---

### Q30. What is token binding?

**Answer:** Cryptographically bind token to a specific client (TLS session, device):
- Token only valid from the original client
- Even if stolen, can't be used from another device
- Implementations: Mutual TLS, DPoP (Demonstration of Proof of Possession)

**💡 Tip:** Token binding = **"this token only works for THIS device."** Even if stolen, useless on another machine. Defense-in-depth against token theft.

---

### Q31. What is mutual TLS (mTLS)?

**Answer:** Both client and server verify each other's certificates:
- Server proves identity (standard TLS)
- Client also proves identity with certificate
- Used in: service-to-service auth, zero-trust networks, banking

More secure than API keys/tokens but harder to manage (certificate lifecycle).

**💡 Tip:** mTLS = **"we both prove who we are."** Standard HTTPS = server proves identity. mTLS = both prove. Used in service mesh (Istio), zero-trust architectures.

---

### Q32. What is the principle of least privilege?

**Answer:** Give users/services only the minimum permissions needed:
- User: only access their own data, not others'
- Service: only access APIs it needs
- Admin: separate permissions for read vs write vs delete
- Token: minimal claims/scope

Regularly audit permissions.

**💡 Tip:** Least privilege = **"only what you need, nothing more."** Viewer shouldn't have delete permission. Microservice shouldn't access unrelated databases. Audit regularly.

---

### Q33. What is credential stuffing?

**Answer:** Attack using leaked username/password combinations from one breach to login elsewhere:
- Prevention: rate limiting, MFA, breach detection APIs, unique passwords
- Tools attackers use: automation scripts, proxy rotation

Encourage users to use password managers and unique passwords.

**💡 Tip:** Credential stuffing = **"trying leaked passwords everywhere."** Same password on 5 sites, one leaks → all 5 compromised. MFA + rate limiting + breach detection = defense.

---

### Q34. What is a security header?

**Answer:** HTTP headers that enhance security:
- `Strict-Transport-Security` (HSTS): force HTTPS
- `Content-Security-Policy` (CSP): prevent XSS
- `X-Frame-Options`: prevent clickjacking
- `X-Content-Type-Options: nosniff`: prevent MIME sniffing
- `Referrer-Policy`: control referrer info

Use `helmet` middleware in Express to set all headers.

**💡 Tip:** Security headers = **browser-level defense.** Helmet in Express sets them all. HSTS = HTTPS only. CSP = no inline scripts. Easy to add, big security wins.

---

### Q35. What is SSO (Single Sign-On)?

**Answer:** One login provides access to multiple applications:
- User logs in once to identity provider (IdP)
- All connected applications trust the IdP
- Protocols: SAML, OIDC, CAS
- Examples: Google Workspace, Azure AD, Okta

**💡 Tip:** SSO = **"login once, access everything."** IdP (identity provider) = gatekeeper. Apps trust IdP. Users love it (one password). Admins love it (centralized control).

---

### Q36. What is SAML?

**Answer:** Security Assertion Markup Language — XML-based SSO protocol:
- Used in enterprise environments
- Identity Provider (IdP) → Service Provider (SP) trust relationship
- User authenticates at IdP → receives SAML assertion → accesses SP

Older than OIDC but still common in enterprise (Active Directory, Okta).

**💡 Tip:** SAML = **enterprise SSO** (XML-based, older). OIDC = **modern SSO** (JSON-based, OAuth2). Most new implementations use OIDC.

---

### Q37. What is a permission vs a role?

**Answer:**
- **Permission**: specific action (`user:delete`, `report:export`)
- **Role**: group of permissions (`admin` = all permissions, `editor` = read + write)

Best practice: assign permissions to roles, roles to users. Check permissions, not roles, in code.

**💡 Tip:** Permission = **atomic action**. Role = **bundle of permissions**. Check permissions in code (not roles) — more flexible. `can('user:delete')` not `if (role === 'admin')`.

---

### Q38. What is OAuth2 scope?

**Answer:** Limits what an access token can do:
```
access_token with scope: "read:profile read:email"
// Can read profile and email, nothing else
```

Token only has permissions for requested scopes. Client requests scopes, user approves.

**💡 Tip:** Scope = **"what this token is allowed to access."** Fine-grained permissions per token. `read:posts` vs `write:posts`. Request minimal scopes.

---

### Q39. What is a replay attack?

**Answer:** Capturing and reusing a valid token/request:
- Defense: timestamps, nonces (one-time values), short token expiration
- JWT defense: `jti` (JWT ID) claim + server-side tracking
- HTTPS prevents capture in transit

**💡 Tip:** Replay = **"use the same request twice."** Short-lived tokens + nonce + timestamp = defense. HTTPS prevents capture in the first place.

---

### Q40. What is the difference between stateless and stateful authentication?

**Answer:**
- **Stateless** (JWT): all info in token, server doesn't store session. Scale easily but can't revoke until expiry.
- **Stateful** (session): server stores session data. Can revoke immediately but needs shared storage for scaling.

Hybrid: JWT + blacklist for revocation. Best of both worlds.

**💡 Tip:** Stateless = **"token has everything"** (scale easy, revoke hard). Stateful = **"server remembers"** (revoke easy, scale harder). Hybrid = JWT + token blacklist.

---

## Advanced Questions (Q41–Q50)

### Q41. What is zero-trust architecture?

**Answer:** "Never trust, always verify" — authenticate and authorize every request:
- No implicit trust based on network location
- Every service-to-service call authenticated
- mTLS, JWT, RBAC at every layer
- Continuous verification, not just at login

**💡 Tip:** Zero trust = **"trust nothing, verify everything."** Not "inside the network = safe." Every request is untrusted until proven otherwise.

---

### Q42. What is DPoP (Demonstration of Proof of Possession)?

**Answer:** Binds access token to a specific public key:
1. Client generates key pair
2. Creates DPoP proof JWT signed with private key
3. Sends proof with every request
4. Server verifies: token + proof match the same key

Prevents token theft — stolen token can't be used without the private key.

**💡 Tip:** DPoP = **"prove you own this token."** Token + private key = access. Stolen token without key = useless. OAuth 2.1 standard.

---

### Q43. What is step-up authentication?

**Answer:** Require additional verification for sensitive operations:
```
Regular action: access token sufficient
Sensitive action (transfer money): require MFA verification first
```

Implemented with: `acr` (Authentication Context Class Reference) claim, `auth_time` claim, or challenge-response.

**💡 Tip:** Step-up = **"prove it again for sensitive stuff."** Logged in with password? Fine for reading. But transferring money? Prove with MFA again.

---

### Q44. How to handle JWT revocation?

**Answer:** JWTs can't be revoked by default (stateless). Solutions:
- **Short expiration**: minimize window (15 min)
- **Token blacklist**: store revoked tokens in Redis (TTL = remaining expiry)
- **Token versioning**: store version per user, reject tokens with old version
- **Refresh token rotation**: detect reuse → revoke all tokens

**💡 Tip:** Short expiry + refresh rotation = **good enough for most apps.** For instant revocation: Redis blacklist. For nuclear option: increment user's token version.

---

### Q45. What is delegated authorization?

**Answer:** User grants a third-party application limited access to their resources:
- "Allow this app to read your Google contacts"
- User delegates specific permissions without sharing credentials
- OAuth2 is the standard for delegated authorization

**💡 Tip:** Delegated auth = **"I allow this app to access my data."** User grants, app accesses, user can revoke. OAuth2 = the protocol for this.

---

### Q46. What is a token introspection endpoint?

**Answer:** OAuth2 endpoint to check if a token is still valid:
```
POST /oauth/introspect
Authorization: Basic base64(clientId:clientSecret)
Body: token=abc123

Response: { "active": true, "scope": "read", "exp": 1234567890 }
```

Used by resource servers to validate tokens (especially opaque tokens).

**💡 Tip:** Introspection = **"is this token still good?"** Call the auth server to verify. Necessary for opaque tokens. For JWT, can validate locally (check signature + expiry).

---

### Q47. What is token binding and sender-constrained tokens?

**Answer:** Tokens bound to a specific client, preventing use if stolen:
- **MTLS token binding**: token bound to client certificate
- **DPoP**: token bound to public key
- **Proof-of-possession**: client must prove ownership

OAuth 2.1 mandates sender-constrained tokens for public clients.

**💡 Tip:** Sender-constrained = **"only the original sender can use this."** Even if intercepted, useless without proof. The future of token security.

---

### Q48. How to implement audit logging for authentication?

**Answer:** Log all auth events for security auditing:
```
{ userId, action: "LOGIN_SUCCESS", ip, userAgent, timestamp, mfaUsed: true }
{ userId, action: "LOGIN_FAILED", ip, reason: "wrong_password" }
{ userId, action: "TOKEN_REFRESH", ip }
{ userId, action: "PERMISSION_DENIED", resource: "/admin/users" }
```

Store immutable logs. Alert on suspicious patterns (multiple failed logins).

**💡 Tip:** Audit logs = **"who did what, when, from where?"** Log all auth events. Alert on anomalies. Required for compliance (SOC 2, HIPAA).

---

### Q49. What is the difference between authentication and identity verification?

**Answer:**
- **Authentication**: proving you are who you claim (password, biometric)
- **Identity verification**: proving your real-world identity (government ID, KYC)
- **Authorization**: what you can do (permissions)

Identity verification = stronger than authentication. Banks, healthcare require both.

**💡 Tip:** Authentication = **"you know the password"** (account level). Identity verification = **"prove you're actually Ali Khan"** (real person). KYC = identity verification.

---

### Q50. How to design a complete auth system?

**Answer:**
1. **Registration**: validate + hash password (bcrypt) + email verification
2. **Login**: verify credentials + issue access + refresh tokens
3. **Token storage**: httpOnly + Secure + SameSite cookies
4. **API auth**: middleware validates token on every request
5. **Refresh**: rotate refresh tokens, detect reuse
6. **Authorization**: RBAC/ABAC checks at route/service level
7. **MFA**: TOTP/webAuthn for sensitive operations
8. **Security**: rate limiting, CORS, helmet, HTTPS
9. **Monitoring**: audit logs, anomaly alerts
10. **Recovery**: password reset with time-limited tokens

**💡 Tip:** Auth = **multiple layers**: bcrypt passwords → JWT tokens → refresh rotation → RBAC → MFA → rate limiting → audit logs. No single point of failure.

---
