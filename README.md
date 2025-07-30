# GCBS Software Engineering Intern - Full-Stack Take-Home Assignment

Hello and welcome\! This assignment is your opportunity to demonstrate your full-stack skills by building a complete web application with Next.js, including database integration and user authentication. We want to see how you approach a problem, design a data model, handle user identity, and make technical decisions.

**Use of AI**: We expect and encourage you to use AI tools (e.g., GitHub Copilot, ChatGPT, Claude). A critical part of the evaluation is documenting your use of these tools in the `README.md` file.

-----

## 📅 Deadline

**Submission Deadline**: 1 August 2025, 23:59 HRS (UTC+6)

Please submit your assignment by the time specified above.

-----

## 🎯 The Challenge: "Quick-Post" Content Aggregator

You will build a multi-user content aggregation web application. The application will allow users to sign in using a **magic link** sent to their email. Once authenticated, they can browse articles from the public **Dev.to API** and save personal bookmarks to their account.

**Third-Party API**: [Dev.to Public API v1 Documentation](https://developers.forem.com/api/v1)

-----

## ✅ Core Requirements

Follow these steps to build the application. We estimate this will take **8-12 hours of focused work**.

### Step 1: Project Setup & Database

1.  **Initialize Project**: Create a new Next.js project using `npx create-next-app@latest`.
      * We recommend using **TypeScript** and **Tailwind CSS**.
2.  **Git Repository**: Set up a **private Git repository** on GitHub and make frequent commits.
3.  **PostgreSQL Database**: Set up a PostgreSQL database. You can use a free-tier cloud provider like [Supabase](https://supabase.com/), [Neon](https://neon.tech/), or a local Docker instance.
4.  **Integrate Prisma & NextAuth.js**:
      * Add Prisma and the **Prisma Adapter for NextAuth.js** to your project.
      * Create a `.env.local` file in your project root to store your `DATABASE_URL` and `NEXTAUTH_SECRET`. **Do not commit this file.**
5.  **Design Database Schema & ERD**:
      * Your `schema.prisma` file must include the required models for the NextAuth.js Prisma Adapter (`User`, `Account`, `Session`, `VerificationToken`).
      * Add a `Bookmark` model. This model **must have a relationship** to the `User` model, indicating that each bookmark belongs to a user.
      * Create an **Entity-Relationship Diagram (ERD)** showing all tables and their relationships. Export it as an image and add it to your repository.

### Step 2: Magic Link Authentication

1.  **Configure NextAuth.js**:
      * Set up the dynamic API route for NextAuth at `pages/api/auth/[...nextauth].ts`.
      * Configure it with the **Prisma Adapter** and the **Email Provider** for passwordless, magic-link authentication. 
2.  **Development Email Handling**:
      * For this assignment, **you do not need to set up a real email-sending service**.
      * Configure the Email Provider to **log the magic link to the console**. This allows you to test the full authentication flow without email server setup. You can achieve this by using the `sendVerificationRequest` function to `console.log` the URL.
      * Hint: use supabase for magiclink

### Step 3: Backend Development (Protected API Routes)

Your API endpoints must now be secure and user-aware.

1.  **Article Proxy**:
      * `GET /api/articles`: Create this public endpoint to proxy requests to the Dev.to API (`https://dev.to/api/articles?per_page=20`).
2.  **Secure Bookmark Endpoints**:
      * All bookmark endpoints must be **protected**. Use `getServerSession` from `next-auth` inside your API routes to get the user's session. If no session exists, return a `401 Unauthorized` error.
      * **List User's Bookmarks**: `GET /api/bookmarks`
          * Fetch and return only the bookmarks associated with the logged-in user's ID.
      * **Add a Bookmark**: `POST /api/bookmarks`
          * When creating a new bookmark, you must associate it with the `userId` from the active session.
      * **Remove a Bookmark**: `DELETE /api/bookmarks/[articleId]`
          * Before deleting, verify that the bookmark with the given `articleId` belongs to the current user.

### Step 4: Frontend Development (UI & User Session)

1.  **Authentication UI**:
      * Create components for signing in and out.
      * The sign-in component should have an email input and a button that triggers NextAuth's `signIn('email', { email })` function.
      * Use the `useSession()` hook from `next-auth/react` to get the user's authentication status.
      * Display the user's email and a "Sign Out" button in the header when they are logged in. Otherwise, show a "Sign In" button.
2.  **Protected Pages**:
      * The `/bookmarks` page must be a **protected route**. Unauthenticated users trying to access it should be prompted to sign in.
3.  **User-Specific Bookmarking**:
      * The bookmarking logic in your `<ArticleCard>` component should now work for the logged-in user, calling the secure API endpoints you built.

### ⭐ Bonus Features (Optional)

  * **UI Feedback**: Use a library like `react-hot-toast` to show notifications for login, logout, and bookmark actions.
  * **Optimistic UI**: When a user clicks "Bookmark," update the UI instantly before the API call completes for a faster perceived response.

-----

## 📖 Documentation: The `README.md` File

Your `README.md` is as important as your code. Please structure it with the following sections.

```markdown
# Quick-Post Content Aggregator

A brief, one-paragraph overview of the project and its purpose.

## How to Run Locally

Provide clear, step-by-step instructions to get the project running.
1. Clone the repository: `git clone ...`
2. Install dependencies: `npm install`
3. Set up environment variables: Copy `.env.example` to `.env.local` and add your `DATABASE_URL` and `NEXTAUTH_SECRET`.
4. Run database migrations: `npx prisma migrate dev`
5. Run the development server: `npm run dev`

## Database Schema (ERD)

Link to the ERD image you created and committed to your repository.

![ERD for Quick-Post](link/to/your/erd_image.png)

Briefly explain the design of your tables and the relationships between them.

## Architectural Decisions & Trade-offs

This is the most important section. Explain *why* you made certain choices.
* **Authentication**: Why is magic link a good choice for this app? What are its security considerations?
* **Database & ORM**: Why did you choose Prisma? What are the limitations or trade-offs of your schema design?
* **Data Fetching**: Why was SSR chosen for the home page and CSR for the bookmarks page?

## AI Usage Log

Be transparent about how you used AI.
* **Example**: "I used ChatGPT to generate the Prisma schema for the NextAuth.js models. **Prompt**: 'Give me the Prisma schema models required for the NextAuth.js Prisma adapter.'"
```

-----

## 📤 Submission Instructions

1.  Ensure all your code, including the ERD image, is pushed to your public GitHub repository.
2.  Make sure the `README.md` file is complete.
3.  Invite the GitHub user `douglasswmcst` to your public repository.
4.  Send an email with the link to your repository to `jamyangtenzin.gcbs@rub.edu.bt `, `dalston.gcbs@rub.edu.bt `, `douglas.cst@rub.edu.bt ` before the deadline.

Good luck\! We are looking forward to reviewing your work.
