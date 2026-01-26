# Digital Garden 🌱

A modern, feature-rich note-taking and knowledge management application built with Next.js and React. Create, organize, and explore your personal digital garden with advanced search, visualization, and analytics.

## Overview

Digital Garden is a fully-featured note-taking application that helps you capture, organize, and discover your thoughts. It features a beautiful modern UI, powerful search capabilities, knowledge graph visualization, and comprehensive analytics—all stored locally in your browser.

### Key Features

- 📝 **Rich Note Taking** - Create and edit notes with Markdown support
- 🏗️ **Collections** - Organize notes into meaningful groups
- 🔗 **Wiki-style Links** - Create `[[note-title]]` references between notes
- 🔍 **Fuzzy Search** - Fast, intelligent search with tag filtering
- 📊 **Analytics Dashboard** - View writing statistics and activity heatmap
- 🌐 **Knowledge Graph** - Interactive visualization of note connections
- ⭐ **Favorites** - Quick access to your most important notes
- 🎲 **Random Discovery** - Serendipitously explore your notes
- 📤 **Export/Import** - Backup and restore your notes as JSON
- 🌙 **Theme System** - Light/dark/system theme support
- ⌨️ **Keyboard Shortcuts** - Power user navigation with Command Palette (Ctrl+K)
- 📱 **Drag & Drop** - Reorder notes with intuitive drag-and-drop
- 🔄 **Version History** - Track note changes and revert to previous versions
- 🔗 **Share Tokens** - Share notes with public access tokens

## Tech Stack

- **Framework**: [Next.js 14](https://nextjs.org/) - React framework for production
- **UI Library**: [React 18](https://react.dev/) - Modern React with hooks
- **Styling**: [Tailwind CSS 4](https://tailwindcss.com/) - Utility-first CSS framework
- **Markdown**: [React Markdown 8](https://github.com/remarkjs/react-markdown) - Markdown rendering
- **Markdown Extensions**: [remark-gfm 3](https://github.com/remarkjs/remark-gfm) - GitHub Flavored Markdown
- **Graph Visualization**: [vis-network 9](https://visjs.org/) - Interactive network visualization
- **Storage**: Browser **localStorage** - Client-side persistence

## Project Structure

```
digigarden/
├── pages/                      # Next.js pages & routes
│   ├── _app.jsx               # Root app wrapper with layout
│   ├── index.jsx              # Home - note list & upload
│   ├── dashboard.jsx          # Analytics & overview dashboard
│   ├── analytics.jsx          # Writing statistics & heatmap
│   ├── collections.jsx        # Collection management
│   ├── graph.jsx              # Knowledge graph visualization
│   ├── search.jsx             # Fuzzy search interface
│   ├── random.jsx             # Random note discovery
│   ├── settings.jsx           # User preferences & theme
│   ├── notes/
│   │   ├── index.jsx          # All notes list
│   │   ├── new.jsx            # Create new note with templates
│   │   ├── [slug].jsx         # View note (dynamic route)
│   │   ├── [slug]/edit.jsx    # Edit note
│   │   └── public/[slug].jsx  # Public note view
│   └── share/[token].jsx      # Share token resolution
│
├── components/                 # Reusable React components
│   ├── CommandPalette.jsx     # Quick navigation (Ctrl+K)
│   ├── DashboardWidget.jsx    # Dashboard card widget
│   ├── DraggableNoteList.jsx  # Drag-and-drop note list
│   ├── editor.jsx             # Markdown editor with preview
│   ├── ErrorBoundary.jsx      # Error handling wrapper
│   ├── ExportImport.jsx       # JSON export/import
│   ├── FavoriteButton.jsx     # Toggle note favorites
│   ├── FileUpload.jsx         # File drag-drop upload
│   ├── HistoryPanel.jsx       # Version history viewer
│   ├── KeyboardShortcuts.jsx  # Shortcuts help modal
│   ├── LoadingState.jsx       # Skeleton loading states
│   ├── Notecard.js            # Individual note card
│   ├── ReadingMode.jsx        # Distraction-free reading
│   ├── TemplateSelector.jsx   # Note templates
│   ├── ThemeCustomizer.jsx    # Theme color picker
│   ├── ThemeToggle.jsx        # Light/dark/system theme
│   └── ToastContext.js        # Global notifications
│
├── lib/                        # Utility functions & libraries
│   ├── storage.js             # localStorage CRUD operations
│   ├── advancedSearch.js      # Search algorithm utilities
│   ├── analytics.js           # Analytics calculations
│   ├── collections.js         # Collection management logic
│   ├── favorites.js           # Favorites functionality
│   ├── fuzzySearch.js         # Fuzzy search implementation
│   ├── linkParser.js          # Wiki-link parsing
│   ├── templates.js           # Note templates
│   ├── themes.js              # Theme management
│   └── writingAnalytics.js    # Writing statistics
│
├── styles/
│   └── globals.css            # Global styles & theme variables
│
├── public/                     # Static assets
└── package.json               # Dependencies & scripts
```

## Data Flow Architecture

```
User Input (UI)
    ↓
Components (React)
    ↓
Event Handlers
    ↓
lib/storage.js (CRUD Layer)
    ↓
Browser localStorage
```

### How It Works

1. **User Creates/Edits Note** → Component state updates
2. **Save Action Triggered** → `saveNote()` called from `lib/storage.js`
3. **Storage Layer** → Generates slug, manages versioning
4. **localStorage** → Persists to browser's local storage
5. **Components Re-render** → UI reflects new data

### Key Storage Functions

```javascript
// lib/storage.js
getAllNotes()          // Fetch all notes
saveNote(note)         // Create or update note
deleteNote(id)         // Delete note
getHistory(noteId)     // Get version history
revertToVersion(...)   // Restore old version
toggleFavorite(id)     // Add/remove favorite
getAllCollections()    // Fetch collections
createShareToken(...)  // Generate share token
resolveShareToken(...) // Access shared note
```

## Installation & Setup

### Prerequisites
- **Node.js** 16.x or higher
- **npm** or **yarn** package manager

### Step 1: Clone/Navigate to Project
```bash
cd d:\PROJECTS\digigarden\digigarden
```

### Step 2: Install Dependencies
```bash
npm install
```

### Step 3: Run Development Server
```bash
npm run dev
```

The application will start at **http://localhost:3000**

### Production Build
```bash
npm run build
npm start
```

## Usage Guide

### Creating Notes
1. Navigate to **Home** or **/notes/new**
2. Choose a template (optional) or start blank
3. Enter title and markdown content
4. Click **Save** (auto-saves every 3 seconds)

### Organizing with Collections
1. Go to **Collections**
2. Create a new collection
3. Add notes to collection from note cards or menu

### Searching Notes
1. Navigate to **Search** page
2. Type query for fuzzy search
3. Filter by tags using the tag selector
4. Sort by relevance, newest, or oldest

### Using the Knowledge Graph
1. Go to **Graph** page
2. Create wiki-links in notes: `[[Note Title]]`
3. View interactive visualization of connections
4. Click nodes to navigate to notes

### Viewing Analytics
1. Navigate to **Analytics**
2. See writing heatmap by year
3. View writing statistics (word count, notes created)
4. Explore topic evolution and readability

### Keyboard Shortcuts
- **Ctrl+K** - Open Command Palette (quick navigation)
- **Ctrl+S** - Save note (when editing)
- **?** - Show keyboard shortcuts help

### Exporting Your Data
1. Go to **Dashboard**
2. Click **Export Notes**
3. Your notes will download as `digital-garden-export-[date].json`

### Importing Notes
1. Click **Import Notes** on Dashboard
2. Select a previously exported JSON file
3. Notes are merged (duplicates by slug are skipped)

### Sharing Notes
1. Edit note and click **Generate Share Token**
2. Share the public URL with others
3. Viewers can access note without login

## Configuration

### Theme Customization (settings.jsx)
- Colors are CSS variables in `styles/globals.css`
- Themes stored in localStorage under `dg_settings`
- Supports light, dark, and system preference

### Local Storage Keys
```javascript
dg_notes          // All notes array
dg_collections    // All collections array
dg_settings       // User preferences
dg_favorites      // Favorite note IDs
```

## Performance Features

- **Client-side Only** - No server dependency, instant loading
- **Lazy Loading** - Components load on demand
- **Optimized Search** - Fuzzy search with debouncing
- **Auto-save** - Saves every 3 seconds while editing
- **Hydration Safe** - Proper SSR handling with Next.js

## Browser Compatibility

Works on all modern browsers with localStorage support:
- ✅ Chrome/Chromium 55+
- ✅ Firefox 55+
- ✅ Safari 11+
- ✅ Edge 15+

## Features Deep Dive

### Collections
- Organize notes into named groups
- Add/remove notes from collections
- View all notes in a collection
- Delete collections (notes preserved)

### Favorites
- Quick-access to important notes
- Dashboard shows favorite notes
- Toggle from note card or menu

### Version History
- Every save creates a version snapshot
- Automatically stores up to 10 versions per note
- Revert to any previous version
- Shows timestamp and word count for each version

### Analytics Dashboard
- Overview widget with total notes and words
- Activity widget showing recent changes
- Top tags widget with tag frequency
- Most linked widget showing connection count

### Writing Heatmap
- Visualizes writing activity by date
- Shows intensity of writing per day
- Grouped by year
- Useful for tracking writing habits

### Command Palette
- Quick search and navigation
- Jump to any note or page
- Searchable commands
- Keyboard-first interface

## Troubleshooting

### Notes Not Saving?
- Check browser console for errors
- Verify localStorage is enabled in browser settings
- Try exporting existing notes as backup

### Search Not Working?
- Clear browser cache
- Reload page (F5)
- Check note titles contain search terms

### Graph Not Displaying?
- Ensure wiki-links are formatted: `[[Note Title]]`
- Check note titles match exactly (case-sensitive)
- Try reloading page

### Theme Not Applying?
- Clear browser cache
- Check localStorage in DevTools
- Verify CSS variables loaded in styles/globals.css

## Future Enhancement Ideas

- 📌 **Tags** - Better tag management and cloud tags
- 🔐 **Encryption** - End-to-end encryption for notes
- ☁️ **Cloud Sync** - Optional cloud backup and sync
- 👥 **Collaboration** - Real-time multi-user editing
- 📱 **Mobile App** - Native mobile application
- 🎨 **Custom Themes** - User-created themes
- 📄 **PDF Export** - Export notes as PDF
- 🌍 **Multi-language** - i18n support

## Resume Highlights

This project demonstrates:
- **Modern React Development** - Hooks, context, component composition
- **State Management** - Custom localStorage layer with proper CRUD
- **UI/UX Design** - Beautiful, intuitive interface with themes
- **Data Visualization** - Interactive graphs using vis-network
- **Performance** - Client-side optimization and lazy loading
- **Architecture** - Clean separation of concerns with lib layer
- **Feature Implementation** - 15+ complex features
- **Problem Solving** - Refactored from Supabase to pure localStorage

## License

Open source and free to use.

---

**Built with ❤️ using Next.js & React**

For questions or feature requests, feel free to explore the codebase or reach out!
