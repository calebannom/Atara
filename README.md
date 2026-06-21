# Atara - Mental Health Companion App

A beautiful, cross-platform mental health tracking app built with React Native, Expo, and Firebase.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [Frontend Architecture](#frontend-architecture)
4. [Backend Setup (Firebase)](#backend-setup-firebase)
5. [State Management](#state-management)
6. [Getting Started](#getting-started)
7. [Project Structure](#project-structure)

---

## Project Overview

Atara helps users:

- Track daily moods with activities and notes
- Write journal entries
- Complete guided mood check-ins
- View mood statistics and trends
- Access mental health resources

---

## Tech Stack

### Frontend

- **Framework**: React Native with Expo SDK
- **Routing**: Expo Router (file-based routing)
- **Styling**: React Native StyleSheet
- **Icons**: Lucide React Native
- **Typography**: PlayfairDisplay & Nunito (Google Fonts)
- **Type Safety**: TypeScript

### Backend

- **Authentication**: Firebase Auth
- **Database**: Cloud Firestore
- **Local Storage**: AsyncStorage (for preferences)

---

## Frontend Architecture

### Screens & Navigation

The app uses Expo Router's file-based routing system:

#### Auth Screens

- `app/onboarding.tsx` - First-time onboarding carousel
- `app/login.tsx` - User login screen
- `app/register.tsx` - User registration screen

#### Main Tabs

- `app/(tabs)/index.tsx` - Home screen with quick actions and daily quote
- `app/(tabs)/journal.tsx` - Journal entries list
- `app/(tabs)/stats.tsx` - Mood statistics and trends
- `app/(tabs)/more.tsx` - Settings and additional options

#### Modals

- `app/modal/mood-checkin.tsx` - Guided mood check-in
- `app/modal/mood-tracker.tsx` - Quick mood logging
- `app/modal/journal-entry.tsx` - Journal entry editor
- `app/modal/discover.tsx` - Articles, videos, breathing exercises

---

### Components

#### Reusable UI Components

- `components/Loader.tsx` - Loading spinner
- `components/ActivityChip.tsx` - Selectable activity tags
- `components/MoodBadge.tsx` - Mood indicator with emoji

---

## Backend Setup (Firebase)

### 1. Firebase Project Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project named "Atara"
3. Enable **Email/Password Authentication**
4. Create a **Cloud Firestore** database (start in test mode for development)

### 2. Firebase Configuration File

The Firebase SDK is initialized in `lib/firebase.ts`:

```typescript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: 'YOUR_API_KEY',
  authDomain: 'YOUR_PROJECT.firebaseapp.com',
  projectId: 'YOUR_PROJECT_ID',
  storageBucket: 'YOUR_PROJECT.appspot.com',
  messagingSenderId: 'YOUR_SENDER_ID',
  appId: 'YOUR_APP_ID',
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
```

### 3. Firestore Database Structure

```
users/
  {userId}/
    displayName: string
    email: string
    createdAt: Timestamp
    streakCount: number

    moods/
      {moodId}/
        mood: number (1-5: awful-rad)
        emoji: string
        label: string
        note: string
        tags: string[] (activities)
        createdAt: Timestamp

    journals/
      {journalId}/
        title: string
        content: string
        mood: number
        tags: string[]
        createdAt: Timestamp
```

---

## State Management

We use React Context API for global state:

### 1. Theme Context (`context/ThemeContext.tsx`)

- Manages light/dark mode
- Persists preference to AsyncStorage
- Provides theme object for components

### 2. Auth Context (`context/AuthContext.tsx`)

- Manages user authentication state
- Handles login, register, logout
- Fetches and caches user profile from Firestore

### 3. Mood Context (`context/MoodContext.tsx`)

- Fetches and caches mood entries
- Adds new mood entries
- Optimistic updates for fast UI response

### 4. Journal Context (`context/JournalContext.tsx`)

- Manages journal entries
- Supports add, edit, delete operations

---

## Getting Started

### Prerequisites

1. Node.js 18+ installed
2. Expo Go app on your phone (for testing)

### Installation

1. Clone or navigate to the project: `cd f:\Atara\Atara`
2. Install dependencies: `npm install`
3. Update Firebase config in `lib/firebase.ts` with your credentials
4. Run the app: `npm run dev`

---

## Project Structure

```
Atara/
├── app/                          # Expo Router screens
│   ├── (tabs)/                   # Main tab navigation
│   │   ├── index.tsx             # Home screen
│   │   ├── journal.tsx           # Journal screen
│   │   ├── stats.tsx             # Statistics screen
│   │   ├── more.tsx              # More screen
│   │   └── _layout.tsx           # Tab navigator
│   ├── modal/                    # Modal screens
│   ├── index.tsx                 # Initial route
│   ├── login.tsx                 # Login
│   ├── register.tsx              # Register
│   └── onboarding.tsx            # Onboarding
├── components/                   # Reusable components
├── context/                      # Context providers
├── lib/                          # Firebase & utility functions
├── constants/                    # Mock data, theme, types
└── assets/                       # Images & icons
```
