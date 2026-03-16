# DigiGarden - Complete Project Workflow 🌱

Your **DigiGarden** is a **full-stack digital note-taking and knowledge management platform** built with Next.js, PostgreSQL, and NextAuth. Here's the complete overview:

---

## **🏗️ Tech Stack**

- **Frontend**: Next.js 14 + React 18
- **Styling**: Tailwind CSS 4
- **Database**: PostgreSQL (via Prisma ORM)
- **Authentication**: NextAuth.js with JWT sessions
- **Markdown**: react-markdown + remark-gfm
- **Graph Visualization**: vis-network
- **Security**: bcryptjs for password hashing

---

## **📋 Core Workflow**

### **1. User Authentication Flow**
```
Landing Page (/) 
    ↓
Register (/auth/register) → Create account → Hash password → Store in PostgreSQL
    ↓
Login (/auth/login) → Verify credentials → Generate JWT session
    ↓
Dashboard (/dashboard) → User-specific view
```

**Key Points:**
- Users must register/login to access any features
- Each user's data is completely isolated (notes, collections, favorites)
- Secure sessions with NextAuth.js
- Password hashing with bcrypt

---

### **2. Notes Management Workflow**

```
Create Note (/notes/new)
    ↓
Optional: Select Template (meeting, journal, research, etc)
    ↓
Write Content (Markdown editor with live preview)
    ↓
Add Tags + Create Wiki Links [[Other Note]]
    ↓
Save → API POST /api/notes → Prisma → PostgreSQL
    ↓
Auto-generate slug from title
    ↓
Note stored in database with userId
```

**CRUD Operations:**
- **Create**: POST `/api/notes` → Inserts into DB
- **Read**: GET `/api/notes` → Fetches all user's notes
- **Update**: POST `/api/notes` → Updates existing note
- **Delete**: DELETE `/api/notes/[id]` → Removes note
- **View**: `/notes/[slug]` → Display single note
- **Edit**: `/notes/[slug]/edit` → Edit mode

---

### **3. Collections System**
```
Create Collection (/collections)
    ↓
Add notes to collection (drag & drop or selection)
    ↓
API POST /api/collections → Store noteIds array
    ↓
View collection → Filter notes by collection
```

**Features:**
- Organize related notes together
- Multiple notes per collection
- Notes can belong to multiple collections
- Delete collection (notes preserved)

---

### **4. Search & Discovery**

**A. Fuzzy Search** (`/search`)
```
User types query
    ↓
Fuzzy search algorithm (lib/fuzzySearch.js)
    ↓
Search in: titles, content, tags
    ↓
Filter by tags
    ↓
Sort by: relevance, newest, oldest
```

**B. Random Discovery** (`/random`)
- Serendipitous note exploration
- Displays random note from your collection

---

### **5. Knowledge Graph** (`/graph`)
```
Parse all notes for [[wiki-links]]
    ↓
Extract note connections (lib/linkParser.js)
    ↓
Build network graph using vis-network
    ↓
Interactive visualization:
  - Nodes = Notes
  - Edges = Links between notes
    ↓
Click node → Navigate to note
```

---

### **6. Analytics Dashboard** (`/dashboard` + `/analytics`)

**Dashboard Widgets:**
- **Overview**: Total notes, total words, avg words/note
- **Activity**: Recent changes and edits
- **Top Tags**: Most used tags with frequency
- **Most Linked**: Notes with most connections
- **Favorites**: Quick access starred notes

**Analytics Page:**
- **Writing Heatmap**: Activity calendar by date
- **Writing Statistics**: Word count, note count
- **Topic Evolution**: Tag trends over time
- **Readability Scores**: Content analysis

---

### **7. Features Matrix**

| Feature | Endpoint/Component | Database Table | Description |
|---------|-------------------|----------------|-------------|
| **Notes** | `/notes/*` | `notes` | Create, edit, view markdown notes |
| **Collections** | `/collections` | `collections` | Group notes together |
| **Favorites** | `/favorites` API | `favorites` | Star important notes |
| **Search** | `/search` | - | Fuzzy search across all notes |
| **Graph** | `/graph` | - | Visualize note connections |
| **Templates** | Component | - | Pre-built note structures |
| **History** | Component | - | Version tracking (not yet in DB) |
| **Themes** | `/settings` | - | Light/dark/custom themes |
| **Export/Import** | Component | - | Backup as JSON |
| **Share Tokens** | `/share/[token]` | - | Public note sharing (not yet in DB) |
| **Command Palette** | Ctrl+K | - | Quick navigation |
| **Drag & Drop** | Component | - | Reorder notes |

---

## **🔄 Data Flow Architecture**

```
User Interaction (Browser)
    ↓
React Component (pages/*)
    ↓
Event Handler (onClick, onSubmit)
    ↓
API Route (pages/api/*)
    ↓
Authentication Check (NextAuth session)
    ↓
Prisma Client (lib/prisma.js)
    ↓
PostgreSQL Database
    ↓
Response → JSON
    ↓
Update React State
    ↓
Re-render UI
```

---

## **🗄️ Database Schema**

```prisma
User (users table)
├── id: UUID
├── email: String (unique)
├── password: String (hashed)
├── name: String?
└── Relations:
    ├── notes[]
    ├── collections[]
    └── favorites[]

Note (notes table)
├── id: UUID
├── userId: UUID (FK → User)
├── title: String
├── content: Text
├── slug: String (unique per user)
├── tags: String[]
├── created_at: DateTime
└── updated_at: DateTime

Collection (collections table)
├── id: UUID
├── userId: UUID (FK → User)
├── name: String
├── description: Text?
├── noteIds: String[]
└── created_at: DateTime

Favorite (favorites table)
├── id: UUID
├── userId: UUID (FK → User)
├── noteId: String
└── created_at: DateTime
```

---

## **🔒 Security Features**

1. **Authentication Required** - All routes protected except landing/login/register
2. **User Isolation** - API routes check `session.user.id` for every query
3. **Password Security** - bcrypt hashing (never store plain text)
4. **JWT Sessions** - Secure token-based authentication
5. **CSRF Protection** - Built into NextAuth
6. **SQL Injection Prevention** - Prisma parameterized queries

---

## **⚡ Key Features in Detail**

### **Markdown Editor**
- Live preview side-by-side
- GitHub Flavored Markdown support
- Auto-save every 3 seconds
- Keyboard shortcut: Ctrl+S

### **Wiki-style Links**
- Syntax: `[[Note Title]]`
- Auto-linking between notes
- Powers knowledge graph

### **Templates**
Pre-built structures for:
- Meeting notes
- Daily journal
- Research paper
- Project plan
- Book notes
- Recipe

### **Command Palette** (Ctrl+K)
- Quick navigation to any page
- Search all notes
- Keyboard-first interface

### **Theme System**
- Light/Dark/System modes
- Custom color picker
- CSS variable-based
- Persisted in localStorage

---

## **📂 Project Structure**

```
pages/
├── index.jsx           → Landing page
├── dashboard.jsx       → Main dashboard
├── analytics.jsx       → Writing analytics
├── collections.jsx     → Collection manager
├── graph.jsx          → Knowledge graph
├── search.jsx         → Fuzzy search
├── random.jsx         → Random note
├── settings.jsx       → User preferences
├── auth/
│   ├── login.jsx      → Login page
│   └── register.jsx   → Registration
├── notes/
│   ├── index.jsx      → All notes list
│   ├── new.jsx        → Create note
│   ├── [slug].jsx     → View note
│   └── [slug]/edit.jsx → Edit note
└── api/
    ├── auth/[...nextauth].js → NextAuth config
    ├── notes/[id].js         → Note CRUD
    ├── collections/[id].js   → Collection CRUD
    └── favorites/index.js    → Favorite CRUD

components/
├── CommandPalette.jsx       → Quick nav (Ctrl+K)
├── DashboardWidget.jsx      → Analytics widgets
├── DraggableNoteList.jsx    → Drag-drop reorder
├── editor.jsx              → Markdown editor
├── ExportImport.jsx        → JSON backup
├── FavoriteButton.jsx      → Star notes
├── FileUpload.jsx          → Drag-drop upload
├── HistoryPanel.jsx        → Version history
├── KeyboardShortcuts.jsx   → Help modal
├── ReadingMode.jsx         → Distraction-free
├── TemplateSelector.jsx    → Note templates
└── ThemeToggle.jsx         → Theme switcher

lib/
├── prisma.js              → Database client
├── storage.js             → (Legacy localStorage)
├── analytics.js           → Analytics calculations
├── collections.js         → Collection logic
├── favorites.js           → Favorites logic
├── fuzzySearch.js         → Search algorithm
├── linkParser.js          → Wiki-link parser
├── templates.js           → Template definitions
└── themes.js              → Theme management
```

---

## **🚀 Development Workflow**

```bash
# Install dependencies
npm install

# Run Prisma migrations
npx prisma migrate dev

# Start dev server
npm run dev

# Build for production
npm run build
npm start
```

**Environment Variables** (`.env`):
```
DATABASE_URL="postgresql://user:password@localhost:5432/digigarden"
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key"
```

---

## **🎯 User Journey Example**

1. **New User** visits `/` → Sees landing page
2. **Registers** at `/auth/register` → Account created
3. **Logs in** at `/auth/login` → Session established
4. **Redirected** to `/dashboard` → Sees empty state
5. **Creates first note** via `/notes/new` → Selects "Journal" template
6. **Writes content** with markdown → Auto-saved
7. **Adds tags** like `#personal #daily` → Organized
8. **Creates collection** "2026 Journal" → Groups notes
9. **Links notes** using `[[Yesterday's Note]]` → Connected
10. **Views graph** at `/graph` → Visualizes connections
11. **Searches** for keyword → Finds relevant notes
12. **Stars favorite** → Quick dashboard access
13. **Exports data** → JSON backup downloaded
14. **Customizes theme** → Dark mode enabled

---

## **🔮 Current Limitations & TODOs**

### **Not Yet Fully Migrated to Database:**
- Version history (still using localStorage)
- Share tokens (still using localStorage)
- Theme preferences (still using localStorage)
- Some analytics data

### **Future Enhancements:**
- Real-time collaboration
- Mobile app
- End-to-end encryption
- Cloud sync
- PDF export
- Multi-language support

---

## **📊 API Endpoints Reference**

### **Authentication**
- `POST /api/auth/register` - Register new user
- `POST /api/auth/[...nextauth]` - NextAuth handlers (login, logout, session)

### **Notes**
- `GET /api/notes` - Get all user's notes
- `POST /api/notes` - Create/update note
- `GET /api/notes/[id]` - Get specific note
- `DELETE /api/notes/[id]` - Delete note

### **Collections**
- `GET /api/collections` - Get all user's collections
- `POST /api/collections` - Create/update collection
- `DELETE /api/collections/[id]` - Delete collection

### **Favorites**
- `GET /api/favorites` - Get all user's favorites
- `POST /api/favorites` - Toggle favorite status

---

## **🎨 Component Architecture**

### **Page Components**
- Handle routing and page-level state
- Fetch data via API calls
- Use Next.js dynamic routing

### **Reusable Components**
- Self-contained UI elements
- Accept props for configuration
- Emit events for parent handling

### **Context Providers**
- ToastContext - Global notifications
- AuthContext - User session state

### **Custom Hooks**
- useDragDrop - Drag and drop functionality
- Built-in Next.js hooks (useRouter, useSession)

---

## **💾 Storage Strategy**

### **PostgreSQL (Primary Storage)**
- User accounts
- Notes content
- Collections
- Favorites

### **localStorage (Legacy/UI State)**
- Theme preferences
- Version history (temporary)
- Share tokens (temporary)
- User settings

### **Migration Status**
- ✅ Users → PostgreSQL
- ✅ Notes → PostgreSQL
- ✅ Collections → PostgreSQL
- ✅ Favorites → PostgreSQL
- ⏳ Version history → To be migrated
- ⏳ Share tokens → To be migrated

---

## **🔧 Configuration Files**

- `next.config.js` - Next.js configuration
- `tailwind.config.ts` - Tailwind CSS settings
- `tsconfig.json` - TypeScript configuration
- `postcss.config.mjs` - PostCSS plugins
- `prisma/schema.prisma` - Database schema
- `.env` - Environment variables

---

## **📝 Development Guidelines**

### **Adding New Features**
1. Create database schema update (if needed)
2. Run Prisma migration
3. Create/update API route
4. Build React component
5. Add to navigation (if page)
6. Test authentication check
7. Update documentation

### **Code Style**
- Use functional components with hooks
- Keep components small and focused
- Extract reusable logic to lib/
- Use async/await for API calls
- Handle loading and error states

### **Security Checklist**
- ✅ Check session in all API routes
- ✅ Validate user owns resource
- ✅ Sanitize user input
- ✅ Use Prisma (prevents SQL injection)
- ✅ Hash passwords (never plain text)
- ✅ Use HTTPS in production

---

This is a **production-ready knowledge management application** with full authentication, database persistence, and 15+ advanced features! 🚀

**Last Updated**: February 13, 2026
