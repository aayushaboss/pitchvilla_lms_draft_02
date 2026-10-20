# Handoff: Pitchvilla Desktop (LMS + Jobsvilla)

## Overview
Desktop (≥1280px) web designs for Pitchvilla:
1. **Pitchvilla LMS** — a learning management system with four role flows: Onboarding (sign up / sign in / college verification), Student, Coordinator (faculty), and Admin (college).
2. **Jobsvilla Desktop** — 1440px desktop adaptation of the college placement app (repo `aayushaboss/pitchvilla_placementapp_frontend`, `lib/`): Sign in, Home feed, Browse, Job detail, Applications, Career Quiz, Profile.

## About the Design Files
The files in `designs/` are **design references built in HTML** — prototypes showing intended look and behaviour, not production code. Recreate them in the target codebase's environment (the existing Flutter app for Jobsvilla; for the LMS, the team's chosen web stack — React/Next.js recommended if none exists) using its established patterns. Open any `.dc.html` in a browser (keep `support.js` beside it). Logic for each screen lives in the `<script data-dc-script>` class at the bottom of each file — read it for mock data, state and transitions.

## Fidelity
**High-fidelity.** Final colours, type, spacing, copy and interactions. Recreate pixel-accurately. All data is mock.

## Entry point
`Pitchvilla LMS.dc.html` — a dark 32px "PROTOTYPE FLOW" bar (prototype-only, do not build) switches between Onboarding / Student / Coordinator / Admin. Role persists in localStorage `pv-lms-role`. Logging out from any portal opens a confirm dialog ("Log out of Pitchvilla?") and returns to Onboarding.

## Shared app shell (Student, Coordinator, Admin)
- Root: `display:flex`, min-height 100vh, min-width 1280px, white background.
- **Sidebar**: 248px fixed width, sticky full height, bg #F5F4F1, right border 1px #E8E6E1, padding 24px 16px 20px, vertical gap 18px.
  - Logo: 12px yellow (#FFCC00) dot + "pitchvilla" (14px/500, letter-spacing −0.3px) + "LMS" tag (10px/500, padding 2px 6px, radius 5px, bg #ECEAE5).
  - Nav item: height 36px, padding 0 12px, radius 10px, gap 14px; ionicon 13px + label 12px. Hover bg #EFEDE8. Active = white bg / weight 500 / #2B2B2B; inactive #6B6966. Count badge: 16px pill, #2B2B2B bg, white 10px/500 tabular numerals.
  - User card pinned bottom: padding 12px, radius 16px, 1px #E8E6E1 border, 36px initials avatar (#ECEAE5), name 12px/500, sub 11px #6B6966, 32px circular log-out button.
- **Content**: flex:1, scrolls independently. Cards: white, 1px #E8E6E1, radius 16px. Inner rows/inputs radius 10px.
- **Modals/overlays**: fixed backdrop rgba(43,43,43,0.32), centered card (white, 1px #E8E6E1, radius 16px, padding 24px, shadow 0 20px 50px rgba(43,43,43,0.14)). Clicking the backdrop closes; clicks inside stop propagation. Buttons right-aligned, gap 8px.
- **Toasts**: dark (#2B2B2B) pill, shadow 0 10px 30px rgba(43,43,43,0.3), auto-dismiss after 2.8s.
- Icons: **Ionicons 7.1** (outline/filled names as in source).

## Screens

### Onboarding (`LMS Onboarding.dc.html`)
Multi-step sign up / sign in: email + password → OTP → choose role (student / coordinator) → search & pick college → profile details (student: name, college ID, programme, dept, semester, batch, phone; coordinator: employee ID, subjects) → upload photo + ID → "under review" status → done (calls `onDone(role, firstLogin)`). Inputs radius 10px; focus ring `0 0 0 3px rgba(255,204,0,0.18)`; error text #B42318 on #FEF3F2; success #3C8200 on #F1F9EA; pending/info #8A6D00 on #FFF6D1.

### Student (`LMS Student.dc.html`)
Nav: Home, My courses, Live classes, Assignments (2), Tests & quizzes, Study materials, Calendar, Community, My profile.
- First login triggers a guided **tour** (step overlays).
- **Course player** (full-screen view): left module drawer (collapsible), video / reading / MCQ modes, right rail toggles Self-study ↔ Live, bookmarks with timestamps, notes, AI tutor chat panel.
- Assignments: submit flow with file/zip upload; **Quiz review** screen with per-question answers.
- Notifications dropdown; search.

### Coordinator (`LMS Coordinator.dc.html`)
Nav: Dashboard, My students, Courses, Live lectures (shows "LIVE" while live), Assignments (17), Quizzes and tests, Study materials, Results, Calendar, Tasks (2), Community, Messages.
- Dashboard: next-lecture countdown (live ticking), tasks, quick actions.
- Live lectures: upcoming/past tabs, **Schedule lecture** overlay, go-live state with live joined counter, **Lecture recording** view, **Post-lecture upload** with progress.
- Courses: opens **Course Builder** (step wizard).
- Overlays: New assignment, Edit assignment, Grade submission (enter marks + feedback → moves to Graded), Study material upload, Quiz details, Preview quiz, Review test answers, New subject, Reassign coordinator.
- Student row → `StudentProfile` (edit details, certificate).

### Admin (`LMS Admin.dc.html`)
Nav: Overview, Requests (pending count), Coordinators, Students, Subjects and modules, Course review (3), Timetable, Results and certificates, Tasks, Announcements, Community, Settings.
- **Requests inbox**: list + detail, checkbox multi-select, approve / bulk approve / auto-approve rules / reject with reason / request info. Student vs coordinator request type toggle.
- Coordinators: overflow menu, **Reassign** modal (pick replacement, decide old coordinator: deactivate etc.), CSV import modal.
- Subject detail, Certificate preview, Student action screens.

### Course Builder (`Course Builder.dc.html`)
Wizard: details (title, description, subject, semester, cover upload) → modules & lessons tree (left) with editor (right) → quizzes (AI-generated question bank, editable count) → review/publish. Modes: create, edit, view, quiz-only. Validation shown after first submit attempt.

### Jobsvilla Desktop (`Jobsvilla Desktop.dc.html`)
Seven 1440px artboards labelled 01–07. Uses `Sidebar.dc.html` (left nav replacing the mobile bottom tab bar) and `JobCard.dc.html`. Browse and Applications are list + detail panes. Separate palette (below). Screen → Flutter file map is in `github.md` of the source project:
- 01 Sign in → landing_screen.dart, login_screen.dart
- 02 Home → college_feed_screen.dart, opportunity_carousel_card.dart
- 03 Browse → opportunity_list_screen.dart, opportunity_filter_screen.dart
- 04 Job detail → opportunity_detail_screen.dart
- 05 Applications → applications_tracker_screen.dart, application_detail_screen.dart
- 06 Career Quiz → career_dna_landing_screen.dart
- 07 Profile → profile_screen.dart

## State (per portal, see source)
- Shell: `role`, `firstLogin`, logout confirm.
- Student: `tab`, `view` (app|player), `mode`, `rail`, `drawer`, module/lesson cursor, `done` lessons, MCQ answers, `submitted` assignments, `tour` step, AI chat, bookmarks, notes.
- Coordinator: `page`, lectures list, `live` + `joined`, upload progress, materials, published items, tasks, grading state, countdown clock (1s interval).
- Admin: `page`, `reqs`, `sel`, `checked`, `modal` (reassign | bulk | auto | reject | info | csv), coordinators, filters.
Replace mock arrays (REQ0, CO0, PAST0, LEC0, ASG…) with API data.

## Design Tokens

### LMS
Colours:
- Ink #2B2B2B · Secondary text #6B6966 · Tertiary #9C9A95 · Disabled #B5B2AC
- Surface #FFFFFF · Sidebar/subtle #F5F4F1 · Hover #F7F6F3 / #EFEDE8 · Chip #ECEAE5 · Border #E8E6E1 · Strong border #D6D3CD / #D1D1CE
- Brand yellow #FFCC00 (primary buttons, ink text on top) · Brand tint #FFF6D1 / rgba(255,204,0,0.25)
- Status (bg / fg): ok rgba(88,204,2,0.12) / #3C8200 · warn rgba(255,149,0,0.14) / #B25900 · bad rgba(255,59,48,0.12) / #D92D20 · neutral #F5F4F1 / #6B6966 · progress green #58CC02
Typography: **Poppins** 400/500 (600 loaded). Sizes 10, 11, 12 (body default), 13, 14, 15, 16 (dialog titles), 18 (page titles), 20, 28/32 (hero numbers). Mono: ui-monospace, Menlo (codes/IDs). Line-height 1.6 for paragraph copy.
Radii: 999px (buttons, pills), 16px (cards, modals), 12px, 10px (nav, inputs, rows), 8px, 5–6px (tags).
Buttons: height 36px, padding 0 16px, pill, 12px/500. Primary #FFCC00; secondary white + 1px #E8E6E1, hover #F7F6F3.
Shadows: modal 0 20px 50px rgba(43,43,43,0.14); toast 0 10px 30px rgba(43,43,43,0.3); small 0 1px 2px rgba(43,43,43,0.2).
Spacing: 4 / 8 / 12 / 16 / 18 / 24 / 32px.
Scrollbars hidden globally.

### Jobsvilla
Ink #1C1410 · Secondary #6B5E54 · Muted #A1A1AA · Surface #F5F5F7 · Border #E5E5EA · Brand #FFCC00 · Success #58CC02 / #3C8200 · Warn #B25900 · Danger #D92D20. Poppins 500/600 (headings 600). Radii 20 (cards), 12, 16, 999. Shadows 0 20px 60px rgba(28,20,16,0.12); yellow CTA glow 0 6px 14px rgba(255,204,0,0.4). Prefer the Flutter app's `lib/theme/*` tokens where they exist.

## Assets
- `designs/assets/module-01-cover.png` — sample course cover.
- Icons: Ionicons (https://ionic.io/ionicons).
- Font: Poppins (Google Fonts).
- Avatars are initials; no other imagery.

## Files
- `designs/Pitchvilla LMS.dc.html` — entry / role switcher
- `designs/LMS Onboarding.dc.html`, `LMS Student.dc.html`, `LMS Coordinator.dc.html`, `LMS Admin.dc.html`
- `designs/Course Builder.dc.html`, `StudentProfile.dc.html` — shared sub-screens
- `designs/Jobsvilla Desktop.dc.html`, `Sidebar.dc.html`, `JobCard.dc.html`
- `designs/support.js` — prototype runtime only (not for production)
