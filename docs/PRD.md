# Engramar - Product Requirements Document (PRD)

## 1. Vision & Goals

### Product Vision
Engramar transforms "English Grammar in Use" by Raymond Murphy from a static textbook into an interactive web-based learning experience. The book is renowned for its clear explanations and comprehensive coverage, but lacks interactive exercises. Engramar eliminates this drawback by providing AI-generated interactive exercises that bring the proven Murphy methodology into the digital age.

### Primary Goal
Enable intermediate English learners to practice grammar interactively through a free, accessible web application that follows the exact structure and pedagogy of the original book.

### Success Criteria
- Users can access units from "English Grammar in Use" online
- Each unit provides clear explanations followed by interactive exercises
- Exercises are varied and engaging, generated using AI (CopilotSDK)
- Users have accounts to track their progress through the units
- The app is free and accessible from any device with a web browser

---

## 2. Target Audience

**Primary Users:** English learners (intermediate level) who want interactive grammar practice but don't necessarily own the physical book

**User Characteristics:**
- Intermediate English proficiency (A2-B2 level)
- Self-motivated learners
- Comfortable using web applications
- Looking for structured grammar practice
- Value free, accessible learning resources

---

## 3. Core Features

### 3.1 Content Structure
- **Organization:** Follow the exact structure and units from "English Grammar in Use"
  - Present tenses
  - Past tenses
  - Present perfect and past
  - Passive
  - Modals
  - Conditionals and wish
  - Reported speech
  - Questions and auxiliary verbs
  - -ing and to infinitive
  - Articles and nouns
  - Pronouns and determiners
  - Relative clauses
  - Adjectives and adverbs
  - Prepositions
  - Phrasal verbs
  - Etc. (145 units total in the original book)

- **MVP Scope:** Start with 10-20 representative units covering major grammar topics
- **Navigation:** Free navigation - users can jump to any unit anytime

### 3.2 Unit Structure
Each unit follows this format:
1. **Grammar Explanation** - Clear, concise explanation of the grammar concept
2. **Examples** - Real-world usage examples
3. **Interactive Exercises** - AI-generated practice exercises

### 3.3 Exercise Types
Implement exercise types progressively, starting with simpler formats:

**Phase 1 (MVP):**
- Multiple choice questions
- Fill-in-the-blank exercises

**Phase 2:**
- Sentence correction
- Drag-and-drop word ordering

**Phase 3:**
- Free-form writing with AI feedback
- Complex scenario-based exercises

### 3.4 AI Exercise Generation
- **Technology:** CopilotSDK
- **Strategy:** Hybrid approach
  - Maintain a pool of pre-generated exercises for fast loading
  - Generate new exercises on-demand for variety and replayability
  - Store successful generated exercises in the pool
- **Quality Control:** Ensure exercises align with the specific grammar point of each unit

### 3.5 User Authentication
- **Method:** Magic link (passwordless email login)
- **Requirement:** Required accounts for all users
- **Benefits:** 
  - Simple, secure authentication
  - No password management burden
  - Progress tracking across devices

### 3.6 Progress Tracking
- **Level:** Simple completion tracking
- **Features:**
  - Track which units are completed
  - Show visual progress indicators
  - Display completion status on unit list
- **No scores or detailed analytics in MVP**

### 3.7 Feedback System
- **Timing:** Show all answers at the end of each exercise
- **Display:**
  - Correct answers highlighted in green
  - Incorrect answers highlighted in red
  - Show the correct answer for each mistake
  - Allow users to review and learn from errors

---

## 4. User Experience

### 4.1 Design Principles
- **Minimalist aesthetic** - Clean, distraction-free interface
- **Content-first** - Grammar explanations and exercises are the focus
- **Responsive design** - Works seamlessly on desktop, tablet, and mobile
- **Fast and lightweight** - Quick page loads, smooth interactions

### 4.2 User Flows

**First-Time User:**
1. Land on homepage with product overview
2. Click "Get Started" or "Sign Up"
3. Enter email address
4. Check email for magic link
5. Click link to authenticate
6. Browse unit list
7. Select a unit
8. Read explanation
9. Complete exercises
10. View results and mark unit as complete

**Returning User:**
1. Click magic link from email (or request new one)
2. Automatically logged in
3. See progress dashboard
4. Continue where they left off or choose new unit

### 4.3 Key Screens
1. **Homepage** - Introduction to Engramar and value proposition
2. **Sign Up / Login** - Email input for magic link
3. **Unit List** - Browse all available grammar units with completion status
4. **Unit Detail** - Grammar explanation, examples, and exercises
5. **Exercise View** - Interactive exercise interface
6. **Results View** - Feedback and correct answers after completion
7. **Profile/Progress** - Simple view of completed units

---

## 5. Technical Approach

### 5.1 Platform
- **Type:** Web application
- **Accessibility:** Works on any device with a modern web browser
- **No native mobile apps in MVP**

### 5.2 Technology Stack (TBD during planning phase)
Considerations:
- Frontend framework for interactive UI
- Backend for authentication and content management
- Database for user data and exercise storage
- CopilotSDK integration for AI exercise generation
- Email service for magic link authentication

### 5.3 Content Management
- Units and grammar explanations stored in database or CMS
- Exercise templates for AI generation
- Pre-generated exercise pool with caching strategy

---

## 6. Monetization

**Model:** Free with all features available

**Rationale:**
- Focus on user acquisition and learning impact
- Remove barriers to education
- Monetization can be reconsidered in future (ads, premium features, donations)

---

## 7. Constraints & Dependencies

### 7.1 Content Rights
- Grammar explanations must be original or properly licensed
- Cannot copy text verbatim from "English Grammar in Use"
- Structure and approach inspired by the book, but content must be created independently

### 7.2 AI Generation Costs
- CopilotSDK usage has associated costs
- Hybrid approach (pre-generated + on-demand) helps manage costs
- Monitor and optimize AI usage as user base grows

### 7.3 MVP Scope Limitation
- Start with 10-20 units (not all 145)
- Implement simpler exercise types first
- Scale content and features based on user feedback

---

## 8. Success Metrics

### For MVP Launch:
- **User Registration:** Number of accounts created
- **Unit Completion Rate:** Percentage of started units that are completed
- **Engagement:** Average units completed per user
- **Technical Performance:** Page load times, exercise generation speed
- **User Feedback:** Qualitative feedback on usability and learning value

### Future Metrics:
- Monthly active users (MAU)
- Retention rates
- Learning outcomes (user-reported improvement)
- Exercise quality ratings

---

## 9. Future Considerations

**Post-MVP Features:**
- Complete all 145 units
- Advanced exercise types (free-form writing, speech recognition)
- Detailed progress analytics and learning insights
- Adaptive difficulty based on user performance
- Community features (forums, peer learning)
- Mobile apps (iOS/Android)
- Additional grammar resources (British vs American English, advanced topics)
- Integration with other language learning tools
- Gamification elements (streaks, badges, leaderboards)

---

## 10. Open Questions

- [ ] Which 10-20 units should be prioritized for MVP?
- [ ] What's the content creation process for grammar explanations?
- [ ] How to ensure AI-generated exercises are high quality and appropriate?
- [ ] What tech stack best fits the requirements?
- [ ] Hosting and infrastructure considerations?
- [ ] Legal review of content approach re: book rights?

---

## Document History

- **Created:** 2026-01-30
- **Version:** 1.0 (Initial PRD)
- **Status:** Draft - Ready for technical planning phase
