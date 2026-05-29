# 🔐 Authentication with Refresh Token

A modern React application implementing a complete JWT authentication flow with automatic **access token refresh** using Axios interceptors — no manual token management required.

---

## 🚀 Features

- **Sign Up / Sign In** — full user registration and login flow
- **JWT Authentication** — access token stored and sent automatically via request interceptors
- **Automatic Token Refresh** — on 401 responses, the refresh token is used to silently renew the session
- **Protected Routes** — `AuthGuard` prevents unauthenticated access to private pages
- **Theme Support** — light/dark mode via `next-themes`
- **Responsive UI** — built with Radix UI primitives and Tailwind CSS

---

## 🛠️ Tech Stack

| Technology                                      | Purpose                       |
| ----------------------------------------------- | ----------------------------- |
| [React 18](https://react.dev/)                  | UI library                    |
| [TypeScript](https://www.typescriptlang.org/)   | Type safety                   |
| [Vite](https://vitejs.dev/)                     | Build tool & dev server       |
| [React Router DOM](https://reactrouter.com/)    | Client-side routing           |
| [Axios](https://axios-http.com/)                | HTTP client with interceptors |
| [React Hook Form](https://react-hook-form.com/) | Form handling                 |
| [Radix UI](https://www.radix-ui.com/)           | Accessible UI primitives      |
| [Tailwind CSS](https://tailwindcss.com/)        | Utility-first styling         |
| [Sonner](https://sonner.emilkowal.ski/)         | Toast notifications           |

---

## 📁 Project Structure

```
src/
├── components/       # Reusable UI components
├── config/           # App configuration (e.g., storage keys)
├── contexts/         # React contexts (Auth, Theme)
├── entities/         # TypeScript entity types
├── hooks/            # Custom hooks (useAuth, ...)
├── lib/              # Utility functions
├── pages/            # Route pages (SignIn, SignUp, Home)
├── Router/           # Route definitions and AuthGuard
├── services/         # API layer (AuthService, OrdersService, httpClient)
└── styles/           # Global styles
```

---

## ⚙️ Getting Started

### Prerequisites

- Node.js `>= 18`
- Yarn or npm
- A running backend API at `http://localhost:3000` with the following endpoints:
  - `POST /signup`
  - `POST /signin`
  - `POST /refresh-token`

### Installation

```bash
# Clone the repository
git clone https://github.com/lucasgomescosta/autentication_refresh_token.git
cd autentication_refresh_token

# Install dependencies
yarn install
```

### Running in Development

```bash
yarn dev
```

The app will be available at [http://localhost:5173](http://localhost:5173).

### Building for Production

```bash
yarn build
```

---

## 🔄 How Token Refresh Works

1. Every request automatically attaches the `accessToken` via an Axios **request interceptor**.
2. If the API returns `401 Unauthorized`, a **response interceptor** catches the error.
3. The interceptor calls `POST /refresh-token` with the stored `refreshToken`.
4. On success, the new tokens are saved and the **original request is retried** transparently.
5. If the refresh itself fails, the user is signed out and redirected to login.

```
Request ──► [Request Interceptor] ──► attach Bearer token ──► API
                                                                │
                                                           401 Error?
                                                                │
                                                    [Response Interceptor]
                                                                │
                                                      POST /refresh-token
                                                                │
                                                    ┌───────────┴───────────┐
                                                  Success               Failure
                                                    │                      │
                                             Retry request            Sign out user
```

---

## 🔑 Environment & Configuration

The API base URL is configured in `src/services/httpClient.ts`:

```ts
export const httpClient = axios.create({
  baseURL: "http://localhost:3000",
});
```

To use a different API URL, update this value or extract it to an `.env` file:

```env
VITE_API_URL=http://localhost:3000
```

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
