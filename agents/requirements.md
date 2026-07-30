# ITADScanner — Project Requirements

## Project Context
**TAGSCAN** (internally ITADScanner) is a web-based tool designed for IT Asset Disposition (ITAD) workflows. It enables technicians to photograph IT equipment, automatically extract identifying information (model number, serial number) using AI, and maintain a searchable inventory of assets being processed for disposition.

## Target Users
- ITAD facility technicians processing incoming equipment
- IT asset managers tracking disposition workflows
- Compliance officers needing chain-of-custody documentation

## Functional Requirements

### FR-1: Authentication (Mock)
- **FR-1.1**: Users can create accounts with email and password
- **FR-1.2**: Users can sign in with existing credentials
- **FR-1.3**: Sessions persist for 24 hours (configurable)
- **FR-1.4**: Users can sign out, clearing their session
- **FR-1.5**: Unauthenticated users are redirected to the auth page
- **FR-1.6**: All data is scoped to the authenticated user

### FR-2: Dashboard
- **FR-2.1**: Display personalized greeting with user email
- **FR-2.2**: Provide "Start Capture" action to begin new asset capture
- **FR-2.3**: Provide "View Inventory" action to access asset table
- **FR-2.4**: Show quick stats: total captures, today's count, last capture time

### FR-3: Capture Flow
- **FR-3.1**: 5-step guided wizard with visual progress indicator
- **FR-3.2**: Step 1 — Category selection (Laptops/Desktops active; Servers, Drives, Network show "Coming Soon")
- **FR-3.3**: Step 2 — Asset details: Type (Laptop/Desktop), Label (optional), Make (from predefined list)
- **FR-3.4**: Step 3 — Photo capture via device camera or file upload fallback
- **FR-3.5**: Step 4 — AI processing simulation: extract model and serial number from photo
- **FR-3.6**: Step 5 — Review and complete: display all fields, allow editing, add disposition fields
- **FR-3.7**: Save completed capture with all fields, image reference, and AI-fill indicators
- **FR-3.8**: Users can navigate back and forward between steps
- **FR-3.9**: Photo step must gracefully handle camera permission denial

### FR-4: AI Extraction (Mock)
- **FR-4.1**: Simulate 2-second processing delay (configurable)
- **FR-4.2**: Generate plausible model names based on selected make
- **FR-4.3**: Generate plausible serial numbers (alphanumeric, 7-12 characters)
- **FR-4.4**: Mark AI-extracted fields with boolean flags (`modelAiFilled`, `serialAiFilled`)
- **FR-4.5**: AI-filled fields display a visual "sparkle" indicator in the UI

### FR-5: Inventory Management
- **FR-5.1**: Display all captures for the current user in a table
- **FR-5.2**: Table columns: #, Type, Label, Make, Model, Serial No, Reason, Disposition, Date, Actions
- **FR-5.3**: Real-time search/filter across all text fields
- **FR-5.4**: Filter by time period: All, Today, This Week
- **FR-5.5**: Inline editing: click a cell to edit, confirm/cancel
- **FR-5.6**: View captured photo in a modal
- **FR-5.7**: Delete captures with confirmation
- **FR-5.8**: AI-filled cells display sparkle badge indicator
- **FR-5.9**: Empty state with call-to-action to start capturing

### FR-6: Data Persistence
- **FR-6.1**: All data stored in localStorage with `itad_` prefix
- **FR-6.2**: Users table: id, email, passwordHash, createdAt
- **FR-6.3**: Captures table: full schema as defined in architecture plan
- **FR-6.4**: Images stored as base64 data URIs, keyed by capture ID
- **FR-6.5**: Session stored in sessionStorage with expiry timestamp

## Non-Functional Requirements

### NFR-1: Performance
- Pages must load instantly (no network requests, no build artifacts)
- Search filtering must respond within 100ms
- UI transitions must be smooth (60fps target)

### NFR-2: Accessibility
- All interactive elements keyboard-accessible
- Form inputs have proper labels
- ARIA attributes for dynamic state
- WCAG AA color contrast on dark backgrounds
- Visible focus indicators

### NFR-3: Responsive Design
- Fully functional at 480px width (mobile)
- Optimized layouts at 768px (tablet) and 1024px+ (desktop)
- Touch-friendly tap targets (minimum 44x44px)

### NFR-4: Browser Support
- Chrome 100+, Firefox 100+, Safari 16+, Edge 100+
- Graceful degradation for camera API (file input fallback)

### NFR-5: Security (Mock)
- Passwords encoded with btoa() (intentionally simple for mock)
- User data isolated by userId in all queries
- Session expires after configured TTL

### NFR-6: Offline Capability
- Entire application works without any network connection
- All assets are local files
- Data persists in browser storage

## Asset Categories

| ID | Display Name | Status |
|---|---|---|
| `laptops_desktops` | Laptops / Desktops | Active |
| `servers` | Servers | Coming Soon |
| `drives` | Drives & Storage | Coming Soon |
| `network` | Network Equipment | Coming Soon |

## Disposition Options
- Recycle
- Resell
- Donate
- Destroy
- Return to Vendor

## Capture Fields

| Field | Source | Required | Notes |
|---|---|---|---|
| Category | User selection | Yes | Step 1 |
| Type | User selection | Yes | Laptop or Desktop |
| Label | User input | No | Asset tag |
| Make | User selection | Yes | From predefined list |
| Model | AI extraction | No | Editable, sparkle if AI-filled |
| Serial No | AI extraction | No | Editable, sparkle if AI-filled |
| Photo | Camera/upload | Yes | Stored as base64 |
| Reason | User input | No | Step 5 |
| Initial Disposition | User selection | No | Step 5 |
| Final Disposition | User selection | No | Step 5 |
| Disposition Date | User input | No | Step 5 |
| Witnessed By | User input | No | Step 5 |
| Certificate of Destruction | User input | No | Step 5 |