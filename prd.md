# Product Requirements Document (PRD)

## Routina — Smart Routine Management for iOS

---

## 1. Product Overview

Routina is a smart routine management iOS application that helps users consistently perform recurring actions — supplements, habits, and daily routines — through flexible scheduling, simple logging, and intelligent reminders. The app prioritizes speed, clarity, and minimal cognitive load with a today-focused interface.

**Platforms:** iPhone, iPad, Apple Watch  
**Minimum OS:** iOS 26.0+  
**Business Model:** One-time purchase at $3.99 USD (no ads, no subscriptions)  
**Localization:** English (en-US), Thai (th-TH)

---

## 2. Problem Statement

People who take supplements, maintain habits, or follow daily routines struggle with:

- Forgetting when actions are due
- Losing track of consistency and streaks
- Managing complex schedules (fixed times, intervals, contextual triggers)
- Syncing routine data across multiple Apple devices
- Cognitive overload from overly complex habit-tracking apps

Routina solves these problems with an event-driven, offline-first approach that delivers the right reminder at the right time with minimal user friction.

---

## 3. Target Audience

- Health-conscious individuals tracking supplements, medications, or vitamins
- People building daily habits (meditation, exercise, hydration)
- Users who want a simple, fast routine tracker without subscription fatigue
- Thai and English-speaking users
- Apple ecosystem users (iPhone + Watch + iPad)

---

## 4. Core Architecture Principles

| Principle | Description |
|-----------|-------------|
| Event-Driven Scheduling | NO background timers. All computations triggered by explicit events (app launch, foreground, data changes, timezone changes). |
| Offline-First | Full functionality without network. Local-first updates with async CloudKit sync. |
| UTC Storage, Local Display | All timestamps stored in UTC. Convert to local timezone only for display. |
| Immutable Logs | Logs are write-once, never modified. Ensures data integrity and simplifies conflict resolution. |
| 64 Notification Budget | iOS limits pending notifications to 64. Rolling scheduling strategy prioritizes soonest occurrences. |
| Progressive Disclosure | Single mode with essential info first, details on demand. No beginner/advanced split. |
| Defensive Programming | Validate all data, handle corruption gracefully, never crash due to invalid data. |

---

## 5. Technology Stack

| Layer | Technology |
|-------|-----------|
| UI Framework | SwiftUI (MVVM) |
| Persistence | SwiftData with @Model macro |
| Cloud Sync | CloudKit private database |
| Concurrency | Swift 6 with strict actor isolation |
| Notifications | UNUserNotificationCenter (local) |
| Voice | Speech framework (SFSpeechRecognizer) |
| Photos | FileManager + iterative JPEG compression |
| Watch | WatchKit + WatchConnectivity |
| Widgets | WidgetKit + App Intents |
| Localization | Localizable.strings (en, th) |
| Testing | XCTest |

---

## 6. Data Model

### 6.1 Routine
The core entity representing a repeatable action.

| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key, immutable |
| name | String | Required |
| icon | String | SF Symbol name (default: "circle.fill") |
| colorHex | String | Hex color (default: "#6366F1") |
| quantity | Int? | Optional target quantity |
| notes | String? | Optional notes |
| scheduleType | ScheduleType | fixed / interval / relative |
| createdAt | Date | UTC |
| updatedAt | Date | UTC, updated on every modification |
| photoPath | String? | Relative path to compressed JPEG |
| nextOccurrence | Date? | Cached value only — source of truth is Schedule + Logs |

### 6.2 Schedule
Timing configuration for a routine.

| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| routineId | UUID | Foreign key to Routine (indexed) |
| type | ScheduleType | fixed / interval / relative |
| times | [DateComponents]? | For fixed schedules (hour, minute) |
| intervalHours | Int? | 1–168 hours for interval schedules |
| relativeType | RelativeType? | afterMeal / beforeMeal / beforeSleep |
| weekdays | [Int]? | 0=Sunday through 6=Saturday |

### 6.3 Stack
A grouping of routines for batch execution.

| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| name | String | Required |
| routineIds | [UUID] | Ordered list of routine references |
| createdAt | Date | UTC |
| updatedAt | Date | UTC |

### 6.4 Log (Immutable)
A completion record. Write-once, never modified.

| Field | Type | Notes |
|-------|------|-------|
| id | UUID | Primary key |
| routineId | UUID | Foreign key to Routine (indexed) |
| timestamp | Date | UTC |
| completionLocalDate | String | "YYYY-MM-DD" format, local calendar |
| quantityDone | Int | Amount completed |
| status | CompletionStatus | completed / skipped / missed |

### Relationships
- Routine → Schedule: one-to-many (cascade delete)
- Routine → Log: one-to-many (cascade delete)
- Stack → Routine: many-to-many via routineIds (no cascade)

---

## 7. Features

### 7.1 Scheduling Engine (Event-Driven)

Three schedule types:

| Type | Behavior | nextOccurrence |
|------|----------|----------------|
| Fixed | Specific times on selected weekdays | Next matching time/weekday after now |
| Interval | Every N hours (1–168) from last completion | Last completed log timestamp + N hours |
| Relative | Contextual (after meal, before sleep) | Always nil — user-triggered |

Recomputation triggers: app launch, foreground, data changes, timezone changes, midnight boundary.

### 7.2 Routine Management

- Create/edit/delete routines with name, icon, color, quantity, notes, photo
- Structured schedule input (pickers, not free text)
- Photo attachment with iterative JPEG compression (~300KB target)
- EXIF metadata stripping for privacy
- Validation: non-empty name, valid SF Symbol, valid hex color

### 7.3 Logging & Completion

- One-tap completion with haptic feedback
- Minute-bucket duplicate detection (same routineId + same minute)
- Cross-device duplicate tolerance (both logs preserved after sync)
- Immutable logs (no updates, only deletion for corrections)
- Retroactive completion with original scheduled time
- Partial completion support (custom quantity)

### 7.4 Stack Management

- Group routines for one-tap batch execution
- Reorderable routine list within stacks
- Transaction-safe multi-routine completion
- Auto-cleanup of deleted routine references
- Empty stacks allowed (with UI warning)

### 7.5 Notification System (64-Budget)

- Rolling scheduling: next 7 days (fixed), next occurrence (interval), none (relative)
- Priority: soonest first; drop furthest if over 64
- Actions: Complete (background), Snooze (5/10/15 min)
- Race condition protection: check for existing log before creating duplicate
- Sync reconciliation on foreground
- Batch scheduling < 500ms

### 7.6 Streak Tracking

- Daily streak count using completionLocalDate (timezone-safe)
- Completion percentage (last 30 days)
- Skipped logs: neutral (don't break streak, don't increment)
- Missed logs: break streak
- Cached with 5-minute expiration

### 7.7 CloudKit Sync

- Private database only (data isolated per iCloud account)
- Last-write-wins conflict resolution using updatedAt
- Offline-first with persistent sync queue
- Exponential backoff retry (1s → 30s max, 6 attempts)
- iCloud account change detection with data clearing
- Sync status indicator (syncing / synced / error / offline)

### 7.8 Voice Search

- Speech framework (English en-US, Thai th-TH)
- Text normalization: lowercase, trim, Thai diacritics, special chars
- Search ranking: exact > prefix > partial > alphabetical
- 10-second recording limit
- Visual feedback: waveform, transcription, result count
- Fallback to manual text search

### 7.9 Quick Add Parser

- Rule-based only (regex + keyword matching, no AI/ML)
- Extracts: name, quantity, schedule type, time
- Priority resolution: interval > fixed > relative
- Preview before saving (confirm / edit)
- English and Thai keyword support
- Graceful fallback: name-only if parsing fails

### 7.10 Apple Watch Companion

- Two tabs: Due Now, Today
- One-tap completion (local-first, async sync)
- Stack execution support
- Haptic feedback patterns (success/failure/warning)
- Complications showing due routine count (15-min refresh)
- Offline mode with sync queue

### 7.11 Widgets

| Widget | Size | Content |
|--------|------|---------|
| Lock Screen (Circular) | Circular | Due count gauge + next routine icon |
| Lock Screen (Rectangular) | Rectangular | Due count + next routine name/time |
| Lock Screen (Inline) | Inline | Next routine name and time |
| Home Screen (Small) | Small | Due count + next routine name |
| Home Screen (Medium) | Medium | Next 3 due routines |
| Home Screen (Large) | Large | Next 6 due routines + completion buttons |

- Quick completion via App Intents (large widget)
- Deep linking: `routina://routine/{uuid}`, `routina://dueNow`
- Timeline refresh every 15 minutes (16 entries for 4 hours)

### 7.12 Missed Routine Handling

- Detection: fixed routines where nextOccurrence date < current date
- Actions: Skip (log as skipped), Reschedule (new time today), Complete retroactively
- Skipped logs don't break streaks
- Interval/relative routines cannot be "missed"

### 7.13 History View

- Logs grouped by date
- Filtering by routine, date range, status
- Pagination for large datasets (10,000+ logs)
- Log deletion for corrections

### 7.14 Settings

- Language selection (English, Thai)
- Log retention (30/90/180/365 days, Forever)
- Data export (JSON)
- Manual sync button with status
- Dark/Light mode selection
- Erase All Data
- Support website and privacy email
- Medical disclaimer

---

## 8. User Interface

### Navigation Structure

```
TabView (4 tabs)
├── Today (primary)
│   ├── Due Now section
│   ├── Later Today section
│   ├── Flexible section (relative routines)
│   └── Missed section
├── Routines
│   ├── Routine List (grouped by schedule type)
│   ├── Routine Detail
│   └── Routine Form (create/edit)
├── History
│   └── Logs grouped by date with filters
└── Settings
    ├── General (language, appearance)
    ├── Data (retention, export, erase)
    └── About (disclaimer, support, privacy)
```

### Design System

| Element | Specification |
|---------|--------------|
| Touch targets | 44×44pt minimum |
| Typography | SF Pro (17pt body, 28pt titles) |
| Icons | SF Symbols v7 (28pt cards, 20pt buttons) |
| Animations | < 300ms, easeInOut, respect Reduce Motion |
| Haptics | Success/Warning/Error patterns |
| Colors | Semantic (auto light/dark adaptation) |
| Contrast | 4.5:1 minimum (WCAG AA) |

### Color Palette

| Role | Light | Dark |
|------|-------|------|
| Primary | #6366F1 | #818CF8 |
| Secondary | #34D399 | #6EE7B7 |
| Accent | #F59E0B | #FCD34D |
| Success | #22C55E | #4ADE80 |
| Error | #EF4444 | #F87171 |
| Background | #F9FAFB | #0F172A |

---

## 9. Accessibility

- VoiceOver with descriptive labels, hints, and values
- Dynamic Type up to 200%
- 4.5:1 contrast ratio (WCAG AA)
- Respect Reduce Motion and Reduce Transparency
- Keyboard navigation support
- Accessibility actions for routine completion/skip/delete
- State change announcements

---

## 10. Performance Targets

| Metric | Target |
|--------|--------|
| App launch (cold) | < 1 second |
| Schedule recomputation | < 200ms |
| Notification batch scheduling | < 500ms |
| User interaction response | < 100ms |
| Haptic feedback | < 50ms |
| Photo compression | < 2 seconds |
| Routine list (1000+ items) | 60fps scrolling |
| History (10,000+ logs) | Paginated, no slowdown |

---

## 11. Privacy & Security

- All data in user's private CloudKit container
- No third-party analytics, crash reporting, or ad networks
- Photo EXIF metadata stripped
- Permissions requested only when needed (microphone, photos, notifications)
- Medical disclaimer on first launch: "Routina is not a medical app and provides reminders only"
- GDPR/CCPA compliant (data export, right to deletion)

---

## 12. Localization

| Language | Locale | Coverage |
|----------|--------|----------|
| English | en-US | Full (fallback) |
| Thai | th-TH | Full |

- All user-facing strings via Localizable.strings
- Localized date/time/number formats
- Pluralization via .stringsdict
- Thai numeral support in Quick Add parser

---

## 13. App Lifecycle

| Event | Actions |
|-------|---------|
| Cold Launch | Init SwiftData → Load prefs → Recompute schedules → Refresh notifications → Sync → Validate stacks |
| Foreground | Recompute schedules → Reconcile notifications → Sync → Refresh UI → Detect timezone/midnight |
| Background | Persist pending changes → Save state → Complete transactions |
| Memory Warning | Clear photo cache → Release cached data |
| Timezone Change | Recompute all schedules → Reschedule notifications → Update display |

---

## 14. Data Migration

- Schema versioning with SwiftData migration plans
- Zero data loss guarantee
- Lightweight migration for simple changes (add optional field)
- Custom migration for complex changes (rename, type change)
- Rollback strategy if migration fails
- Progress UI during migration

---

## 15. Success Metrics

| Metric | Target |
|--------|--------|
| Daily active usage | User opens app at least once daily |
| Completion rate | > 70% of scheduled routines completed |
| Streak maintenance | Average streak > 7 days |
| App Store rating | ≥ 4.5 stars |
| Crash-free sessions | > 99.9% |

---

## 16. Current Implementation Status

**Completed:** 32 of 36 tasks including all core features — models, repositories, scheduling engine, notifications, logging, streaks, CloudKit sync, photo manager, voice search, quick add, UI views, Watch app, widgets, lifecycle integration, timezone handling, accessibility, localization, error handling, migration, and performance optimizations.

**Remaining:** Final integration checkpoints and polish (Tasks 18, 23, 28, 34–36).

**Widget Setup:** 40% complete — code written, pending Xcode target configuration (model file sharing, App Groups, CloudKit, URL scheme).

---

## 17. Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| 64 notification limit | Rolling scheduling with priority sorting; in-app warning if exceeded |
| CloudKit sync conflicts | Last-write-wins with updatedAt; immutable logs avoid log conflicts |
| Timezone drift | UTC storage + local display; event-driven recomputation |
| Large datasets | Lazy loading, pagination, indexed queries, NSCache |
| Watch connectivity | Local-first approach; async sync; offline queue |
| Schema migration | Versioned migrations; rollback strategy; data export before migration |

---

## 18. Future Considerations

- Additional languages beyond English and Thai
- iPad-optimized layouts
- Siri Shortcuts integration
- Health app integration (with appropriate disclaimers)
- Routine templates and sharing
- Statistics and analytics dashboard
