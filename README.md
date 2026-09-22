# Schedule Planner

Schedule Planner is a team-built capstone product developed for CSE 5914 at The Ohio State University. It gives students one place to build weekly schedules, search for course sections, account for recurring personal commitments, compare generated schedule options, and review backend-powered difficulty and workload analysis.

This repository contains the Angular frontend and its integration with Firebase Authentication and the project's REST API. The backend implementation is not included here.

## Project Overview

The project approaches schedule planning as more than placing courses on a calendar. Students can combine academic and personal commitments, explore multiple candidate schedules, inspect course-level ratings, and request alternatives when a schedule does not meet their needs.

The frontend supports the complete user workflow: Google authentication, profile management, schedule creation and persistence, course and section selection, recurring events, calendar visualization, analysis results, and recommendation previews.

## Key Features

- Google Sign-In through Firebase Authentication, with backend user synchronization
- Create, edit, save, delete, and favorite multiple schedules
- Search for course sections by course number, campus, and academic term
- Select a specific section or add a course manually when section data is unavailable
- Add recurring personal events with descriptions, times, and days of the week
- Visualize courses and events in an interactive weekly calendar
- Adjust the visible time range and include or hide weekends
- Generate and compare multiple candidate schedule options
- Review schedule difficulty, estimated weekly workload, and total credit hours
- Open detailed course ratings with workload and difficulty dimensions
- Request course replacement recommendations and preview proposed schedules side by side
- View the next five upcoming courses or events from a favorite schedule
- Build for server rendering or deploy the browser bundle with Docker and Nginx

## Schedule Analysis and Recommendations

The frontend integrates four backend-powered planning workflows.

### Candidate Schedule Generation

The schedule builder submits the selected courses, recurring events, campus, and term to the backend. Returned schedule candidates appear as selectable options that users can compare, keep, or discard before saving.

### Schedule Analysis

Users can submit a schedule for analysis and review the returned:

- Difficulty score
- Estimated weekly workload
- Total credit hours
- Schedule summary
- Per-course difficulty ratings

The results are presented directly alongside the weekly calendar so users can evaluate schedule structure and workload together.

### Course Ratings

Users can request additional details for an individual course and, when available, its instructor. The interface supports:

- Overall difficulty and estimated weekly time commitment
- Rigor, pace, assessment intensity, and project intensity
- Prerequisites and co-requisites
- Descriptive tags
- Confidence values
- Evidence snippets returned by the backend

Course-rating responses are cached in memory to avoid duplicate requests during the same application session.

### Course Replacement Recommendations

The alteration workflow lets users select courses they want to replace and describe what they want from an alternative. The frontend sends the current schedule, replacement targets, saved user preferences, and custom criteria to the backend. It then converts the returned recommendations into previewable schedule options and displays the original and proposed calendars side by side before applying a change.

The frontend does not run an AI model directly, and the backend implementation is not included in this repository. The code confirms integration with schedule generation, analysis, course-rating, and recommendation endpoints, but it does not establish which AI, machine-learning, or rule-based techniques the backend uses.

## Tech Stack

### Frontend

- Angular 20
- TypeScript with strict compiler settings
- Angular standalone components
- Angular Signals and RxJS
- Angular Material
- SCSS and CSS Grid
- Angular Router
- Angular SSR with an Express server entry point

### Authentication and Integration

- Firebase Authentication
- Google Sign-In
- Angular `HttpClient`
- REST API integration
- Browser local storage for the current application user

### Tooling and Deployment

- Angular CLI
- ESLint and Prettier
- Jasmine and Karma
- Docker multi-stage build
- Nginx static hosting and SPA fallback configuration

## Architecture

```text
                             +-------------------------+
                             | Firebase Authentication |
                             |     Google Sign-In      |
                             +------------+------------+
                                          |
                                          v
+------+       +---------------------------+---------------------------+
| User | ----> |                    Angular Frontend                    |
+------+       |  Pages / Components -> Signals / Services -> Models  |
               +---------------------------+---------------------------+
                                           |
                                           | REST API
                                           v
               +-------------------------------------------------------+
               | External Backend                                      |
               |                                                       |
               | Users and profiles   | Schedule persistence           |
               | Course search        | Schedule generation            |
               | Course ratings       | Analysis and recommendations   |
               +-------------------------------------------------------+
```

`BackendService` centralizes HTTP requests for users, schedules, course search, generation, analysis, ratings, and recommendations. `ScheduleService` manages the active schedule, saved schedules, favorite selection, and unsaved frontend state. Components consume these services through Angular dependency injection and use Signals for reactive UI state.

The project includes Angular server-rendering configuration through Express. The provided Docker setup builds the Angular application and serves its browser output through Nginx.

## Main Pages

| Page | Route | Responsibilities |
| --- | --- | --- |
| Login | `/login` | Google authentication and backend account synchronization |
| Dashboard | `/landing` | Favorite schedule, upcoming items, weekly calendar, and schedule summary |
| My Schedules | `/schedules` | Saved schedule management, editing, deletion, and favorite selection |
| New Schedule | `/schedule/new` | Schedule creation, course search, events, generation, and analysis |
| Edit Schedule | `/schedule/edit/:id` | Editing and saving an existing schedule |

## Screenshots

The repository does not currently include screenshot assets. The following real product views would best document the completed workflow:

1. Dashboard with a favorite weekly schedule and upcoming items
2. Schedule Builder with courses and recurring personal events
3. Course search and section-selection dialog
4. Generated candidate schedule options
5. Schedule difficulty and workload analysis
6. Detailed course-rating dialog
7. Original-versus-recommended schedule comparison

Future screenshots can be stored under `docs/images/` and embedded in this section.

## My Contribution

My primary responsibility was frontend development. I owned major frontend workflows across schedule creation, management, visualization, analysis, and recommendation experiences, integrating these interfaces with backend REST APIs.

My frontend work included the Angular UI, schedule builder, weekly calendar visualization, course and section selection, recurring event management, analysis and recommendation flows, and the client-side service layer connecting these experiences to authentication and backend data.

This was a collaborative capstone product. Backend services, schedule-generation logic, recommendation logic, and project-wide outcomes were team efforts and are not presented here as individual work.

## Team and Capstone Context

Schedule Planner was developed as a complete team capstone product for CSE 5914 at The Ohio State University, rather than as a standalone UI exercise. The repository reflects integration across authentication, persistent user data, course information, schedule management, analysis, and deployment concerns.

After the capstone concluded, the project was selected for a **$6,000 university funding offer** to support continued development. The team did not accept or use the funding because the members graduated and did not continue the project.

## Local Setup

### Prerequisites

- Node.js 20 or later
- npm
- A Firebase project with Google Sign-In enabled
- Access to a backend implementing the API contracts used by this frontend

### Installation

```bash
git clone https://github.com/EDDLK/Schedule-Planner-UI.git
cd Schedule-Planner-UI
npm install
```

Review `src/app/environment.ts` and configure:

- `firebaseConfig` for the Firebase project used by the application
- `apiBaseUrl` for a compatible Schedule Planner backend

Start the development server:

```bash
npm start
```

Open [http://localhost:4200](http://localhost:4200).

Create a production build:

```bash
npm run build
```

The compiled application is written to `dist/Schedule-Planner-UI/`.

### Docker

Build and run the Nginx image:

```bash
docker build -t schedule-planner-ui .
docker run --rm -p 8080:80 schedule-planner-ui
```

Open [http://localhost:8080](http://localhost:8080).

## Repository Scope

This repository contains the frontend application and its API integration layer. It does not contain:

- Backend source code
- Database schema or migrations
- Schedule-generation or recommendation algorithms
- AI or machine-learning model code
- Production usage, accuracy, or performance metrics

Those boundaries are intentional in this README: every technical claim above is based on the frontend implementation contained in this repository or on the documented capstone context.
