# 🌊 Oasis AI - AI-Powered Essay Writing Platform 

**Oasis** is a modern, AI-powered essay writing and evaluation platform that helps users improve their writing skills through personalized feedback, typing practice, and progress tracking.

##  Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Firebase Setup](#-firebase-setup)
- [Gemini API Setup](#-gemini-api-setup)
- [Usage Guide](#-usage-guide)
- [Key Components](#-key-components)
- [Data Flow](#-data-flow)
- [Development](#-development)
- [Contributing](#-contributing)
- [License](#-license)

##  Features

### Core Features

- ** AI Essay Topic Generation**
  - Generate personalized essay topics based on difficulty level and category
  - Categories: General, Technology, Environment, Society, Education, Health, Politics, Science, Arts, Business
  - Difficulty levels: Beginner, Intermediate, Advanced

- ** Essay Writing Interface**
  - Distraction-free writing environment
  - Real-time word and character count
  - Built-in timer to track writing time
  - Save drafts and submit for evaluation
  - Writing guidelines and tips sidebar

- ** AI-Powered Essay Evaluation**
  - Comprehensive scoring system (0-100)
  - Detailed breakdown:
    - Grammar (20 points)
    - Structure (20 points)
    - Relevance (30 points)
    - Word Count (10 points)
  - Grammatical error detection with corrections
  - Structural feedback and suggestions
  - Strengths and areas for improvement

- ** Typing Speed Test**
  - AI-generated typing practice text
  - Real-time WPM (Words Per Minute) calculation
  - Accuracy tracking
  - Error detection and highlighting
  - 60-second timed tests

- ** Progress Tracking**
  - Dashboard with key statistics
  - Writing streak tracking
  - XP (Experience Points) system
  - Average score calculation
  - Recent essays overview
  - Detailed progress analytics

- ** User Authentication**
  - Email/password authentication
  - Google Sign-In integration
  - Secure user sessions
  - Protected routes

##  Tech Stack

### Frontend
- **Framework**: Next.js 15.2.4 (App Router)
- **Language**: TypeScript 5
- **UI Library**: React 19
- **Styling**: Tailwind CSS 3.4.17
- **UI Components**: Radix UI (shadcn/ui)
- **Icons**: Lucide React
- **Animations**: Tailwind CSS Animate

### Backend & Services
- **Database**: Firebase Realtime Database
- **Authentication**: Firebase Authentication
- **AI Service**: Google Gemini AI (gemini-1.5-flash)
- **Storage**: Firebase Storage (configured)

### Development Tools
- **Package Manager**: pnpm
- **Linting**: ESLint (Next.js)
- **Type Checking**: TypeScript

## 📁 Project Structure

```
Oasis-ai/
├── app/                          # Next.js App Router pages
│   ├── api/                      # API routes
│   │   └── evaluate-essay/       # Essay evaluation endpoint
│   ├── auth/                     # Authentication pages
│   │   ├── signin/              # Sign in page
│   │   └── signup/              # Sign up page
│   ├── dashboard/                # Main dashboard (protected)
│   ├── evaluation/               # Essay evaluation pages
│   │   └── [id]/                # Dynamic evaluation route
│   ├── profile/                  # User profile & progress
│   ├── typing/                   # Typing test page
│   ├── write/                    # Essay writing page (protected)
│   ├── layout.tsx                # Root layout
│   ├── page.tsx                  # Landing page
│   └── globals.css               # Global styles
├── components/                   # React components
│   ├── ui/                       # shadcn/ui components
│   ├── smart-logo.tsx           # Logo component
│   └── theme-provider.tsx       # Theme context provider
├── hooks/                        # Custom React hooks
│   └── useAuth.ts               # Authentication hook
├── lib/                          # Utility libraries
│   ├── config.ts                # Gemini AI configuration
│   ├── firebase.ts              # Firebase initialization
│   ├── gemini.ts                # Gemini AI functions
│   └── utils.ts                 # Utility functions
├── public/                       # Static assets
│   ├── images/                  # Image files
│   └── video/                   # Video files
├── styles/                       # Additional styles
├── package.json                 # Dependencies
├── tailwind.config.ts           # Tailwind configuration
├── tsconfig.json                # TypeScript configuration
└── README.md                    # This file
```

##  Getting Started

### Prerequisites

- Node.js 18+ installed
- pnpm installed (`npm install -g pnpm`)
- Firebase account
- Google Cloud account (for Gemini API)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Oasis-ai
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables** (see [Environment Variables](#-environment-variables))

4. **Configure Firebase** (see [Firebase Setup](#-firebase-setup))

5. **Configure Gemini API** (see [Gemini API Setup](#-gemini-api-setup))

6. **Run the development server**
   ```bash
   pnpm dev
   ```

7. **Open your browser**
   ```
   http://localhost:3000
   ```

##  Environment Variables

Create a `.env.local` file in the root directory:

```env
# Firebase Configuration
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_auth_domain
NEXT_PUBLIC_FIREBASE_DATABASE_URL=your_database_url
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_storage_bucket
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# Gemini API Key
NEXT_PUBLIC_GEMINI_API_KEY=your_gemini_api_key
```

**Note**: Currently, the Gemini API key is hardcoded in `lib/config.ts`. For production, move it to environment variables.

##  Firebase Setup

1. **Create a Firebase Project**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Click "Add Project"
   - Follow the setup wizard

2. **Enable Authentication**
   - Navigate to Authentication → Sign-in method
   - Enable Email/Password authentication
   - Enable Google Sign-In provider

3. **Set up Realtime Database**
   - Navigate to Realtime Database
   - Create database (start in test mode for development)
   - Copy the database URL

4. **Get Firebase Config**
   - Go to Project Settings → General
   - Scroll to "Your apps" section
   - Copy the Firebase configuration object

5. **Update `lib/firebase.ts`**
   ```typescript
   const firebaseConfig = {
     apiKey: "your-api-key",
     authDomain: "your-auth-domain",
     databaseURL: "your-database-url",
     projectId: "your-project-id",
     storageBucket: "your-storage-bucket",
     messagingSenderId: "your-sender-id",
     appId: "your-app-id",
   }
   ```

6. **Set Database Rules** (for production)
   ```json
   {
     "rules": {
       "essays": {
         "$uid": {
           ".read": "$uid === auth.uid",
           ".write": "$uid === auth.uid"
         }
       },
       "evaluations": {
         "$uid": {
           ".read": "$uid === auth.uid",
           ".write": "$uid === auth.uid"
         }
       }
     }
   }
   ```

##  Gemini API Setup

1. **Get API Key**
   - Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
   - Create a new API key
   - Copy the API key

2. **Update Configuration**
   - Option 1: Update `lib/config.ts` directly (development only)
   - Option 2: Use environment variable (recommended for production)
     ```typescript
     const apiKey = process.env.NEXT_PUBLIC_GEMINI_API_KEY
     ```

3. **Model Configuration**
   - Current model: `gemini-1.5-flash`
   - Can be changed in `lib/config.ts`

##  Usage Guide

### For Users

1. **Sign Up / Sign In**
   - Navigate to `/auth/signup` or `/auth/signin`
   - Create account with email/password or use Google Sign-In

2. **Write an Essay**
   - Go to Dashboard → Click "Start New Essay"
   - Select difficulty level and category
   - Click "Generate Essay Topic"
   - Start writing when ready
   - Click "Submit for Review" when finished

3. **View Evaluation**
   - After submission, you'll be redirected to evaluation page
   - Review scores, feedback, and grammatical errors
   - Navigate back to dashboard to see updated stats

4. **Take Typing Test**
   - Go to Dashboard → Click "Typing Test"
   - Click "Start Test" and type the displayed text
   - View your WPM and accuracy results

5. **Track Progress**
   - View dashboard for quick stats
   - Visit Profile page for detailed analytics
   - Check writing streak and XP progress

### For Developers

#### Adding a New Feature

1. Create component in `components/` directory
2. Add route in `app/` directory (if needed)
3. Update navigation in layout/header components
4. Add Firebase data structure (if storing data)

#### Modifying AI Prompts

- Essay topic generation: `lib/gemini.ts` → `generateEssayTopic()`
- Essay evaluation: `lib/gemini.ts` → `evaluateEssay()`
- Typing text generation: `lib/gemini.ts` → `generateTypingText()`

##  Key Components

### Pages

- **`app/page.tsx`**: Landing page with features overview
- **`app/dashboard/page.tsx`**: Main dashboard with stats and recent essays
- **`app/write/page.tsx`**: Essay writing interface with topic generation
- **`app/evaluation/[id]/page.tsx`**: Detailed essay evaluation view
- **`app/typing/page.tsx`**: Typing speed test interface
- **`app/profile/page.tsx`**: User progress and analytics

### Hooks

- **`hooks/useAuth.ts`**: Authentication state management
  - Provides: `user`, `loading`, `signIn`, `signUp`, `signInWithGoogle`, `logout`

### Libraries

- **`lib/firebase.ts`**: Firebase initialization and exports
- **`lib/gemini.ts`**: Gemini AI integration functions
- **`lib/utils.ts`**: Utility functions (JSON parsing, class merging)

##  Data Flow

### Essay Writing Flow

```
User → Select Topic (Difficulty + Category)
  ↓
Generate Topic (Gemini AI)
  ↓
Write Essay (Timer + Word Count)
  ↓
Submit Essay → Save to Firebase
  ↓
Evaluate Essay (Gemini AI via API)
  ↓
Save Evaluation → Firebase
  ↓
Display Results → Evaluation Page
```

### Authentication Flow

```
User → Sign Up/Sign In
  ↓
Firebase Authentication
  ↓
Set User Session
  ↓
Protected Routes Check
  ↓
Load User Data (Essays + Evaluations)
  ↓
Display Dashboard
```

### Data Structure

**Essays** (`/essays/{essayId}`):
```json
{
  "userId": "user123",
  "title": "Essay Topic Title",
  "content": "Essay content...",
  "wordCount": 450,
  "timeSpent": 1800,
  "isDraft": false,
  "createdAt": 1234567890,
  "updatedAt": 1234567890,
  "topic": {
    "title": "Topic Title",
    "category": "Technology",
    "difficulty": "Intermediate",
    "targetWords": "400-500 words",
    "timeLimit": "45 minutes"
  }
}
```

**Evaluations** (`/evaluations/{evaluationId}`):
```json
{
  "essayId": "essay123",
  "userId": "user123",
  "overallScore": 85,
  "breakdown": {
    "grammar": 18,
    "structure": 17,
    "relevance": 25,
    "wordCount": 8
  },
  "grammaticalErrors": [...],
  "feedback": {...},
  "createdAt": 1234567890
}
```

##  Development

### Available Scripts

```bash
# Development server
pnpm dev

# Production build
pnpm build

# Start production server
pnpm start

# Run linter
pnpm lint
```

### Code Style

- Use TypeScript for type safety
- Follow Next.js App Router conventions
- Use functional components with hooks
- Follow Tailwind CSS utility-first approach
- Comment complex logic (already added to key files)

### File Naming Conventions

- Components: PascalCase (`DashboardPage.tsx`)
- Utilities: camelCase (`useAuth.ts`)
- Pages: lowercase (`page.tsx`)
- Routes: lowercase with brackets (`[id]`)

##  Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Write clear, commented code
- Follow existing code structure
- Test your changes thoroughly
- Update documentation if needed
- Ensure TypeScript types are correct

## Notes

### Current Limitations

- Writing streak is hardcoded (needs daily activity calculation)
- Daily challenge feature is placeholder ("Coming Soon")
- Gemini API key is hardcoded (should use env variables)
- No pagination for essays list
- No search/filter functionality

### Future Enhancements

- [ ] Real-time collaborative writing
- [ ] Export essays as PDF/DOCX
- [ ] Social features (share essays)
- [ ] Advanced analytics and charts
- [ ] Mobile app version
- [ ] Offline mode support
- [ ] Multiple language support
- [ ] Custom essay prompts

##  License

This project is licensed under the MIT License - see the LICENSE file for details.

##  Author

**Ananya Shukla**
**Abuzar Khan**

- Project: Oasis AI Platform

##  Acknowledgments

- [Next.js](https://nextjs.org/) - React framework
- [Firebase](https://firebase.google.com/) - Backend services
- [Google Gemini AI](https://ai.google.dev/) - AI capabilities
- [shadcn/ui](https://ui.shadcn.com/) - UI components
- [Tailwind CSS](https://tailwindcss.com/) - Styling framework

---

**Made with for better writing**
