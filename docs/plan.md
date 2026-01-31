# Engramar - Technical Implementation Plan

## Problem Statement
Build an interactive web-based grammar learning application that transforms "English Grammar in Use" concepts into an engaging digital experience with AI-generated exercises. The MVP will cover Units 1-10 (Present tenses basics) with multiple choice and fill-in-the-blank exercises.

## Technical Decisions Summary

### Stack & Architecture
- **Backend:** ASP.NET Core Web App (Razor Pages)
- **Architecture:** Clean Architecture with dependency inversion
- **Database:** SQLite with Entity Framework Core (direct DbContext injection)
- **Authentication:** ASP.NET Core Identity with magic link (passwordless)
- **Email Service:** SendGrid (free tier)
- **CSS Framework:** Bootstrap (minimalist theme, light mode only)
- **Navigation:** Traditional Razor Pages (full page reloads)
- **Logging:** Serilog (structured logging)
- **Testing:** Unit + Integration tests (xUnit/NUnit)
- **Deployment:** CI/CD pipeline (GitHub Actions or Azure DevOps)
- **Hosting:** TBD (decision deferred)

### Content Strategy
- **Units:** Start with Units 1-10 from the book
- **Explanations:** Markdown files read at startup (original content written by you)
- **Exercises per Unit:** 5-10 exercises
- **Exercise Storage:** Database with JSON for options/answers
- **AI Strategy:** Seed some exercises, generate more on demand (hybrid)
- **Quality Control:** Manual review of each generated exercise

### User Experience
- **Exercise Presentation:** All exercises shown together on one page
- **Layout:** Side-by-side (explanation left, exercises right on desktop)
- **Results Display:** Inline summary (X/Y correct) + colored highlights
- **Completion Tracking:** Automatic after viewing results
- **Retakes:** Yes, same exercises initially + "Get Fresh Exercises" button
- **Progress:** Simple completion tracking (unit done/not done)

### Constraints
- **Budget:** $0-50/month (hobby project)
- **Developer:** Solo (you)
- **Timeline:** No specific deadline, iterative development
- **Performance:** Not critical for MVP, functionality first
- **Compliance:** Minimal user data to reduce compliance burden
- **Anti-abuse:** Not for MVP
- **Content Rights:** Medium concern, will write original content carefully

---

## Project Structure (Clean Architecture)

```
Engramar/
├── src/
│   ├── Engramar.Web/                 # Razor Pages, UI layer
│   │   ├── Pages/
│   │   │   ├── Index.cshtml          # Homepage
│   │   │   ├── Auth/
│   │   │   │   ├── Login.cshtml      # Magic link request
│   │   │   │   └── Verify.cshtml     # Magic link handler
│   │   │   ├── Units/
│   │   │   │   ├── Index.cshtml      # Unit list
│   │   │   │   └── Detail.cshtml     # Unit explanation + exercises
│   │   │   └── Profile/
│   │   │       └── Index.cshtml      # User progress
│   │   ├── wwwroot/                  # Static assets (CSS, JS, images)
│   │   ├── appsettings.json
│   │   └── Program.cs
│   │
│   ├── Engramar.Application/         # Business logic, services
│   │   ├── Services/
│   │   │   ├── IUnitService.cs
│   │   │   ├── UnitService.cs
│   │   │   ├── IExerciseService.cs
│   │   │   ├── ExerciseService.cs
│   │   │   ├── IExerciseGeneratorService.cs
│   │   │   ├── ExerciseGeneratorService.cs (CopilotSDK)
│   │   │   ├── IEmailService.cs
│   │   │   ├── EmailService.cs (SendGrid)
│   │   │   ├── IProgressService.cs
│   │   │   └── ProgressService.cs
│   │   ├── DTOs/
│   │   │   ├── UnitDto.cs
│   │   │   ├── ExerciseDto.cs
│   │   │   └── ProgressDto.cs
│   │   └── Interfaces/               # Repository/service abstractions
│   │
│   ├── Engramar.Domain/              # Core domain models
│   │   ├── Entities/
│   │   │   ├── User.cs
│   │   │   ├── Unit.cs
│   │   │   ├── Exercise.cs
│   │   │   ├── UserProgress.cs
│   │   │   └── MagicLinkToken.cs
│   │   └── Enums/
│   │       └── ExerciseType.cs (MultipleChoice, FillInTheBlank)
│   │
│   └── Engramar.Infrastructure/      # Data access, external services
│       ├── Data/
│       │   ├── ApplicationDbContext.cs
│       │   ├── Migrations/
│       │   └── Seed/
│       │       └── SeedData.cs
│       ├── ContentLoaders/
│       │   └── MarkdownUnitLoader.cs # Reads markdown files
│       └── ExternalServices/
│           └── CopilotSdkClient.cs
│
├── tests/
│   ├── Engramar.UnitTests/
│   └── Engramar.IntegrationTests/
│
├── content/                           # Markdown content files
│   └── units/
│       ├── unit-01.md
│       ├── unit-02.md
│       └── ...
│
├── docs/
│   └── PRD.md
│
└── README.md
```

---

## Database Schema

### Users Table (ASP.NET Identity)
- Standard Identity tables (AspNetUsers, AspNetRoles, etc.)
- Additional fields: EmailVerified, CreatedAt

### Units Table
```
- Id (int, PK)
- UnitNumber (int)
- Title (string)
- Content (text) - loaded from markdown
- CreatedAt (datetime)
```

### Exercises Table
```
- Id (int, PK)
- UnitId (int, FK)
- Type (enum: MultipleChoice, FillInTheBlank)
- QuestionText (string)
- OptionsJson (json) - for multiple choice options
- CorrectAnswer (string)
- ExplanationText (string, nullable)
- IsGenerated (bool) - AI-generated vs seed
- IsReviewed (bool) - manual review flag
- CreatedAt (datetime)
```

### UserProgress Table
```
- Id (int, PK)
- UserId (string, FK to AspNetUsers)
- UnitId (int, FK)
- IsCompleted (bool)
- CompletedAt (datetime, nullable)
- LastAccessedAt (datetime)
```

### MagicLinkTokens Table
```
- Id (int, PK)
- Email (string)
- Token (string, unique)
- ExpiresAt (datetime)
- IsUsed (bool)
- CreatedAt (datetime)
```

---

## Implementation Workplan

### Phase 1: Project Setup & Foundation
- [ ] Create ASP.NET Core Web App with Razor Pages
- [ ] Set up Clean Architecture project structure (4 projects)
- [ ] Configure Entity Framework Core with SQLite
- [ ] Add ASP.NET Core Identity for authentication
- [ ] Set up Serilog for logging
- [ ] Configure User Secrets for local development
- [ ] Add Bootstrap and create base layout (minimalist light theme)
- [ ] Set up GitHub repository and initial CI/CD pipeline
- [ ] Create basic homepage with product overview

### Phase 2: Content Management & Data Layer
- [ ] Design database schema (Users, Units, Exercises, UserProgress, MagicLinkTokens)
- [ ] Create EF Core entity models in Domain project
- [ ] Create ApplicationDbContext in Infrastructure project
- [ ] Generate initial EF Core migration
- [ ] Implement MarkdownUnitLoader to read markdown files
- [ ] Write original content for Units 1-10 as markdown files
- [ ] Create seed data for units (load from markdown)
- [ ] Test database initialization and content loading

### Phase 3: Authentication System
- [ ] Implement magic link token generation service
- [ ] Create EmailService with SendGrid integration
- [ ] Build Login page (email input form)
- [ ] Build Verify page (magic link handler)
- [ ] Implement token validation and user authentication
- [ ] Add email templates for magic link
- [ ] Configure ASP.NET Identity cookie settings
- [ ] Test full authentication flow
- [ ] Add user registration on first login

### Phase 4: Unit Display & Navigation
- [ ] Create Units/Index page (unit list with card-based grid)
- [ ] Implement UnitService in Application layer
- [ ] Display unit cards with completion status
- [ ] Create Units/Detail page (unit explanation + exercises)
- [ ] Implement side-by-side layout (explanation left, exercises right)
- [ ] Add responsive design for mobile (stack vertically)
- [ ] Load and render markdown content
- [ ] Add navigation between units

### Phase 5: Exercise Generation & Management
- [ ] Integrate CopilotSDK for AI exercise generation
- [ ] Implement ExerciseGeneratorService
- [ ] Create prompts for multiple choice generation
- [ ] Create prompts for fill-in-the-blank generation
- [ ] Generate seed exercises for Units 1-10 (5-10 per unit)
- [ ] Manually review all generated exercises
- [ ] Mark reviewed exercises in database
- [ ] Implement ExerciseService for retrieval
- [ ] Add "Get Fresh Exercises" functionality

### Phase 6: Interactive Exercise Interface
- [ ] Build exercise display component (multiple choice)
- [ ] Build exercise display component (fill-in-the-blank)
- [ ] Render all exercises together on one page
- [ ] Add form for user answers
- [ ] Implement client-side form validation
- [ ] Create submit button and POST handler
- [ ] Calculate results (correct/incorrect)
- [ ] Display inline results with colored highlights (green/red)
- [ ] Show correct answers for mistakes
- [ ] Test exercise flow end-to-end

### Phase 7: Progress Tracking
- [ ] Implement ProgressService
- [ ] Track unit completion automatically after viewing results
- [ ] Update UserProgress table
- [ ] Display completion badges on unit list
- [ ] Create Profile/Index page showing completed units
- [ ] Add progress summary (X of 10 units completed)
- [ ] Test progress persistence across sessions

### Phase 8: Error Handling & Polish
- [ ] Create custom error pages (404, 500)
- [ ] Implement global exception handling
- [ ] Add logging for critical operations
- [ ] Add loading states for slow operations
- [ ] Improve form validation messages
- [ ] Add confirmation for "Get Fresh Exercises"
- [ ] Test error scenarios
- [ ] Optimize database queries

### Phase 9: Testing
- [ ] Set up xUnit test projects
- [ ] Write unit tests for services (UnitService, ExerciseService, ProgressService)
- [ ] Write unit tests for exercise generation logic
- [ ] Write integration tests for authentication flow
- [ ] Write integration tests for database operations
- [ ] Test magic link expiration
- [ ] Test exercise answer validation
- [ ] Achieve reasonable test coverage

### Phase 10: Deployment Preparation
- [ ] Finalize hosting decision (Azure/AWS/other)
- [ ] Configure production environment variables
- [ ] Set up production database
- [ ] Configure SendGrid production account
- [ ] Update CI/CD pipeline for deployment
- [ ] Add database migration automation
- [ ] Configure logging for production (file/cloud)
- [ ] Create deployment documentation
- [ ] Perform security review (HTTPS, secrets, etc.)

### Phase 11: MVP Launch
- [ ] Deploy to production environment
- [ ] Run smoke tests on production
- [ ] Verify magic link email delivery
- [ ] Test full user journey on production
- [ ] Monitor logs for errors
- [ ] Share with initial test users
- [ ] Gather feedback
- [ ] Document known issues and future improvements

---

## Key Technical Considerations

### CopilotSDK Integration
- Store API key in User Secrets (local) and environment variable (production)
- Create structured prompts that include:
  - Grammar concept being tested
  - Difficulty level (intermediate)
  - Exercise type (multiple choice vs fill-in-blank)
  - Number of options for multiple choice
  - Context/examples
- Handle API failures gracefully (retry logic, fallback to cached exercises)
- Monitor API usage to stay within budget ($0-50/month)

### Magic Link Authentication
- Generate secure random tokens (GUID or cryptographic random)
- Set expiration (15-30 minutes recommended)
- Include user email in link for verification
- Mark tokens as used after first click
- Handle expired/invalid tokens with clear messaging
- Send welcome email after first successful login

### Exercise Generation Strategy
1. **Initial Seed:** Pre-generate 5 exercises per unit, manually review, mark as reviewed
2. **On-Demand:** When user clicks "Get Fresh Exercises", generate 5-10 new exercises
3. **Caching:** Store generated exercises in database for reuse
4. **Review Queue:** Flag new AI-generated exercises for review before wide usage
5. **Cost Control:** Limit generation frequency per user/session if needed

### Content Loading
- Read markdown files from `/content/units/` at application startup
- Parse markdown to HTML for display (use Markdig library)
- Cache parsed content in memory for performance
- Support hot-reload in development mode

### Responsive Design
- Desktop (>768px): Side-by-side layout (explanation 40%, exercises 60%)
- Tablet/Mobile (<768px): Stack vertically (explanation on top, exercises below)
- Card grid: 3 columns desktop, 2 columns tablet, 1 column mobile
- Bootstrap responsive utilities for layout adjustments

### Performance Optimization
- Eager loading for related entities (Unit + Exercises)
- Index on UserProgress (UserId, UnitId) for fast lookups
- Cache unit content in memory
- Minimize database queries in page handlers
- Defer non-critical loading (analytics, etc.)

---

## Risks & Mitigations

### Risk: Copyright infringement
**Mitigation:** Write completely original explanations, cite structure inspiration, avoid copying any text from the book

### Risk: AI generation costs exceed budget
**Mitigation:** Aggressive caching, limit generation frequency, use seed exercises as fallback, monitor usage closely

### Risk: AI-generated exercises are low quality
**Mitigation:** Manual review of all exercises, iterative prompt improvement, feedback mechanism to flag bad exercises

### Risk: SendGrid free tier limits (100 emails/day)
**Mitigation:** Implement rate limiting on magic link requests, upgrade to paid tier if needed ($15/month for 40k emails)

### Risk: SQLite scalability limitations
**Mitigation:** SQLite is sufficient for MVP (thousands of users), migrate to PostgreSQL/SQL Server if user base grows

### Risk: Solo development burnout
**Mitigation:** Iterative approach, celebrate small wins, no deadline pressure, focus on learning and enjoyment

---

## Success Criteria for MVP

- [ ] 10 units with original grammar explanations deployed
- [ ] 50+ exercises (5+ per unit) reviewed and ready
- [ ] Users can sign up with email and receive magic link
- [ ] Users can browse units and view explanations
- [ ] Users can complete exercises and see results
- [ ] Progress is tracked and persists across sessions
- [ ] Users can retake units and get fresh exercises
- [ ] Application is deployed and accessible via URL
- [ ] No critical bugs or broken flows
- [ ] Basic analytics/logging in place

---

## Future Enhancements (Post-MVP)

### Content Expansion
- Add Units 11-20 (Past tenses)
- Eventually cover all 145 units
- Add example sentences and usage notes
- Include audio pronunciation for examples

### Exercise Types
- Sentence correction (Phase 2)
- Drag-and-drop word ordering (Phase 2)
- Free-form writing with AI feedback (Phase 3)
- Speech recognition for pronunciation practice
- Scenario-based exercises

### User Experience
- Adaptive difficulty based on performance
- Recommendations for what to study next
- Detailed progress analytics (time spent, weak areas)
- Streak tracking and reminders
- Dark mode toggle
- Gamification (badges, achievements)

### Technical Improvements
- Migrate to PostgreSQL for scalability
- Add Redis for caching
- Implement HTMX for smoother interactions
- Add real-time notifications
- Build mobile apps (iOS/Android)
- Rate limiting and anti-abuse measures
- GDPR compliance features (data export/deletion)

### Community Features
- Discussion forums per unit
- Peer review of user-submitted exercises
- Leaderboards
- Study groups
- Teacher/student accounts

---

## Notes

- Keep the focus on clean, maintainable code given solo development
- Document decisions and architecture for future reference
- Prioritize shipping over perfection for MVP
- Gather user feedback early and iterate
- Maintain separation of concerns (Clean Architecture benefits)
- Write tests for critical business logic
- Monitor costs closely given budget constraints

---

## Open Questions to Resolve During Implementation

1. Which markdown parser library? (Markdig recommended)
2. Exact CopilotSDK API endpoints and usage patterns?
3. SendGrid account setup and API key generation?
4. Final hosting platform selection and setup?
5. Domain name registration?
6. CI/CD pipeline specifics (GitHub Actions vs Azure DevOps)?
7. Production logging destination (file system, cloud service)?
8. Backup strategy for SQLite database?

---

## Document History

- **Created:** 2026-01-31
- **Version:** 1.0
- **Status:** Ready for implementation
- **Next Step:** Begin Phase 1 - Project Setup & Foundation
